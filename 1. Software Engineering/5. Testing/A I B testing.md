Суть исследования заключается в том, чтобы разделить аудиторию на равные части и показать им разные версии одной и той же страницы. Чтобы найти ту, которая принесет наиболее высокий эффект, например, конверсию в целевое действие.

A/B testing is a way to compare two versions of something to figure out which performs better. (Harvard Business Review)

#### Feature flagging and randomisation

Feature flagging is used to enable or disable a feature for a given user. And we can think of each AB test as a flag that determines which variant a user sees. That’s where randomisation comes in. 

#### **Naive version of randomisation**: 

1. **Lookup a user’s variant from the database**. If it exists, use the recorded variant. If it doesn’t exist, then…
2. **Roll the dice**. Execute`Math.random()` (or whatever randomisation function exists for your language)
3. **Assign the variant**. Compare the number returned from `Math.random()` . If it’s < 0.5, then assign the control variant. Otherwise assign the test variant.
4. **Save the variant to the database** (so it can be looked up in step 1 the next time the user visits the home page)
5. **Use the variant to determine what copy to render** on the home page
6. **Render the home page**

##### Downsides:
1. **Hard dependency on reading from and writing to a transactional database** If you have a test running on your home page, there’s potentially a lot of write volume going to your testing table. And if you have two, three, four or more tests running on that page, then multiply that load. The app is also trying to read from this table with every page load so there’s a lot of read/write contention. We crashed our app a few times at Storyblocks due to database locks related to this high write volume, particularly when a new test is added to a high traffic page.
2. **Primarily works with traditional architecture where the server renders an HTML page**. It doesn’t really work for single page or mobile apps very well because every test you’re running would need a blocking network call before it could render its content appropriately.
3. **Race condition**. If a user opens your home page multiple times at the same time (say from a Chrome restore), then it’s indeterminant which variant the user is going to see and have recorded. This is because the lookups from step 1 may all return nothing, so each page load rolls the dice and assigns a different variant. It’s random which one actually wins the race to be saved to the database and the user is potentially served different experiences.

#### **Improved randomisation via hashing**

Instead of simply rolling the dice using `Math.random()` , we hash the combination of the experiment identifier and the user identifier using something like MD5, which effectively creates a consistent random number for the experiment/user combination.

![Pasted image 20231014140337](../../_Attachments/Pasted%20image%2020231014140337.png)

After you’ve computed the variant, you log the result, but instead of writing to a transactional database, which is blocking, you write the result to a data firehose (such as [AWS Kinesis](https://aws.amazon.com/pm/kinesis/)) in a non-blocking way. Eventually the data makes its way into a table in your data lake/warehouse for analysis (often called the “assignments” table).

##### Downsides:

1. **There’s still a dependency on a database/service to store the test configuration**. The naive implementation requires fetching the data each time you need to randomise a user into a variant. This is challenging for SPAs and mobile apps.
	==> ! We can have test configuration in cache to avoid DB calls and made it faster. 

2. **There’s no good way to opt users in to a specific variant**. This is often necessary for testing or rollout purposes (e.g., someone should be able to test each variant). The reason for this is that with only hashing, the variant is fully determined by the experiment/user combination.

#### **The answer to randomisation: [Feature flagging](Feature%20flagging)**

Modern feature flagging is essentially a [JSON delivery service](https://www.geteppo.com/blog/introduction-to-feature-flagging-and-randomization). The server maintains a file describing the feature flags and the rules that govern assignment; it's up to the _client_ to download the file and decide which group it belongs to.

There are two kinds of latency relevant to a feature-flagging service:
1. _Evaluation latency_. How long will it take for a client to decide which flag value applies to it?
2. _Update latency_. How long will it take for updated rules to reach the client?

As with most things in engineering, architecture represents tradeoffs. A "smart server" architecture, where the client polled the server every time it evaluated a feature flag, would have near-instant update latency, but poor evaluation latency (and high server costs).

=> Use cache (Google Cloud CDN)

![Pasted image 20231014143431](../../_Attachments/Pasted%20image%2020231014143431.png)
**Update** latency (taken together, the average update latency comes out to about 4 minutes.):
1. **Polling frequency** refers to how often the client SDK requests an update from the servers. This number can be controlled by the customer, and is set to 5 minutes by default (with some jitter added to prevent an accidental DDoS).
2. **Cache time-to-live** **(TTL)** refers to how long the cached file is considered valid. This number is set to 3 minutes, and is not (currently) configurable by the customer.

#### Metrics

The naive approach here is simply to join your assignments table to other tables in your database to compute metric values for each of the users in your experiment.

```SQL
SELECT a.user_id, a.variant, SUM(p.revenue) AS revenue  
FROM assignments a  
JOIN purchases p  
ON a.user_id = p.user_id  
WHERE a.experiment_id = 'some-experiment'  
AND p.purchased_at >= a.assigned_at  
  
SELECT a.user_id, a.variant, COUNT(*) AS num_page_views  
FROM assignments a  
JOIN page_views p  
ON a.user_id = p.user_id  
WHERE a.experiment_id = 'some-experiment'  
AND p.viewed_at >= a.assigned_at
```

##### Downsides:
1. As your user base grows, the ad hoc joins to your assignments table become repetitive and expensive
2. As the number of metrics grows, the SQL to compute them becomes hard to manage (just imagine having 50 or even 1000 of these)
3. If the underlying data varies with time, it becomes hard to reproduce results

#### Solution:
1. The ability to compute the assignments for a particular experiment once per computation cycle
2. A repository to manage the SQL defining your metrics
3. An immutable event or “fact” layer to define your underlying primitives to compute your metrics

An important aspect of configuring an experiment is defining **primary and guardrail metrics**. 

The primary metric for an experiment is the metric most closely associated with what you’re trying to test. So for the homepage refresh of YourDelivery where you’re testing blue vs red background colors, your primary metric is probably revenue. 
Guardrail metrics are things that you typically aren’t trying to change but you’re going to measure them to make sure you don’t negatively impact user experience. Stuff like time on site, page views, etc.

#### Statistics

*Let’s assume we’re running the home page test for YourDelivery shown above, with two variants control (blue) and test (red) with an even 50/50 split between them. Let’s also assume we’re only looking at one metric, revenue. Every user that visits the home page will be assigned to one of the variants and then we can compute the revenue metric for each user. How do we determine if there’s a statistically significant difference between test and control?*
##### **The naive approach: the Student t-test**

The naive approach is simply to use a Student t-test to check if there’s a statistical difference. You compute the mean and standard deviation for test and control, plug them into the t-statistic formula, compare that value to a critical value you look up, and voila, you know if your metric, in this case revenue, is statistically different between the groups.

##### Downsides:

1. **The “[Peeking Problem](https://www.evanmiller.org/how-not-to-run-an-ab-test.html)”**. The classic t-test only guarantees its statistical significance if you look at the results once (e.g., a fixed sample size). 
2. **P-values are notoriously prone to mis-interpretation**, have arbitrary thresholds (e.g., 5%), and do not indicate effect size.
3. **Absolute vs relative differences**. The classic t-test looks at absolute differences instead of relative differences.

##### **[Relative lifts](https://medium.com/jonathans-musings/ab-testing-101-5576de6466b#:~:text=to%20quote%20from-,Eppo%E2%80%99s%20documentation,-on%20the%20subject)**

Look at relative lifts instead of absolute differences.

The rationale behind relative lifts is straight forward: we typically care about relative changes instead absolute changes and they’re easier to discuss. It’s easier to understand a “5% increase in revenue” compared to a “$5 increase in revenue per user”.

##### **[Sequential confidence intervals](https://medium.com/jonathans-musings/ab-testing-101-5576de6466b#:~:text=refer%20you%20to-,Eppo%E2%80%99s%20documentation,-on%20the%20subject)**

##### **[Controlled-Experiment Using Pre-Experiment Data (CUPED)](https://medium.com/jonathans-musings/ab-testing-101-5576de6466b#:~:text=testing%20platforms%20like-,Eppo,-provide%20CUPED%20implementations)**

##### **[Bayesian statistics](https://medium.com/jonathans-musings/ab-testing-101-5576de6466b#:~:text=the%20sections%20of-,Eppo%E2%80%99s%20documentation,-on%20Bayesian%20analysis)**

To learn more - the book [_Bayesian Statistics the Fun Way_](https://www.amazon.com/Bayesian-Statistics-Fun-Will-Kurt/dp/1593279566/)_._ 


### **References:**
1. _[Процесс запуска и проведения АВ-тестов](https://habr.com/ru/companies/tele2/articles/708782/)_
2. _[Использование методов А/Б тестирования. Решение практического кейса в Python](https://habr.com/ru/articles/708872/)_
3. _[Проверка корректности А/Б тестов](https://habr.com/ru/companies/X5Tech/articles/706388/)_
4. _[Без A/B — результат XЗ, или Как мы построили платформу A/B-тестов в Ozon / Евгений Пак (Ozon)](https://www.youtube.com/watch?v=UoHn3sVKvMg) (video)_
5. _[Байесовский подход к АБ тестированию](https://habr.com/ru/companies/glowbyte/articles/732024/)_
6. _[When You Should Prefer “Thompson Sampling” Over A/B Tests](https://towardsdatascience.com/when-you-should-prefer-thompson-sampling-over-a-b-tests-5e789b480458)_
7. [AB Testing 101](https://medium.com/jonathans-musings/ab-testing-101-5576de6466b)