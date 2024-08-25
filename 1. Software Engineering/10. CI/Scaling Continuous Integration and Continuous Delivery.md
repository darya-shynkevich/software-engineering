An unreliable CI system blocks engineers from merging and deploying code, which can be particularly disastrous if there is an outage.

### **What makes CI unreliable?**

[Flaky tests](Flaky%20tests.md) / infrastructure causes an otherwise healthy main to seem unhealthy.
#### **The CI flakiness problem (and mitigations)**
##### Upstream dependency flakiness

To limit the number of dependencies:
1. use [Yarn offline mirroring](https://classic.yarnpkg.com/blog/2016/11/24/offline-mirror/) to install node_modules from tarballs stored in our Git repo so that we aren’t downloading dependencies from the network each time.
2. bake as many dependencies as possible into the Docker image that we use to run CI jobs.
##### [Test flakiness](Flaky%20tests.md) and solutions to solve it

#### **The CI speed problem (and mitigations)**

[Selective testing](Flaky%20tests.md) helped us keep our CI speed down. By only testing the packages that were touched by a PR, we could run fewer tests. Still, some PRs ended up touching a frequently imported package, and would cause us to run the entire test suite.

To improve performance in these cases, parallelize the testing across different CI-job-running shards. To fairly distribute package testing across shards it is possible to collect historical metrics on each package’s test time and then use the metrics to group tests into batches. 
	(-) parallelization means paying for speed with money (more EC2 instances)
	(-) parallelization will plateau on the longest-running operation (see [Amdahl’s Law](https://en.wikipedia.org/wiki/Amdahl%27s_law)). Even if it possible to afford to run 1000 shards, each shard would have to perform CI job setup (preparing the Git repo, setting up the Docker environment), compile the packages it’s tasked with testing, run the tests, emit test metrics, and do the CI job cleanup

#### **The merge race problem**

Merge race conditions happen, where it’s possible for two PRs to have a conflict in main, even though they both passed CI independently.

1. PR1 renames package original to final. CI passes.
2. PR2 adds logic that uses package original. CI also passes.
3. PR1 is merged into main.
4. PR2 is merged into main. The code fails to compile because package original no longer exists, it’s been renamed to final.
5. main is broken until somebody puts up a PR to fix the issue, either by reverting one of the PRs or opening a new PR.

**Solution: merge queues.**

A **merge queue** is exactly what it sounds like: instead of merging directly into main, your PR gets placed on a queue of PRs, and the queue merges changes into main one at a time. BUT If there are many PR’s ahead of yours in the queue, and many are failing, it may take many (partial) CI runs for your PR to actually get merged, and of course, your PR runs the risk of flaking itself, leading to greater frustration.
![](Pasted%20image%2020240623134324.png)

PR Note that even with zero flakes, a merge queue inherently increases CI time. Before adding the PR to the merge queue, it has to pass CI because we want to avoid polluting the queue with broken PRs.

- Run 1: Put up PR, run CI. If it passes, you can add it to the merge queue.
- Run 2: The PR rebases on the queue and runs CI again at least once, possibly more times due to PR’s ahead of it failing.
##### The middle-ground solution: automatic rebasing of risky PRs

To determine which PRs present a merge race risk, use a heuristic based on base commit age. 
Rules are currently:
1. if creating or pushing to your PR => automatically rebase your PR for you if its base commit is more than 12 hours behind main’s HEAD commit. This all happens in seconds so that it’s barely noticeable => can rebase more aggressively without impacting developer velocity.
2. when merging in your PR => automatically rebase your PR for you if its base commit is more than 72 hours behind main’s HEAD commit.

The automatic rebasing is implemented by a webhook-based worker that listens to GitHub PR-related webhook events.

# References:

1. ~~[Improving CI/CD with a Focus on Developer Velocity](https://www.samsara.com/blog/improving-ci-cd-with-a-focus-on-developer-velocity)~~
2. ~~[It’s always greener on the other pipeline](https://medium.com/checkr/its-always-greener-on-the-other-pipeline-5139ed849aef)~~
3. [**Scaling Continuous Integration and Continuous Delivery**](https://www.synopsys.com/blogs/software-security/agile-cicd-devops-glossary/)
4. [Continuous Integration Stack at Facebook](https://code.fb.com/web/rapid-release-at-massive-scale/)
5. [Continuous Integration with Distributed Repositories and Dependencies at Netflix](https://medium.com/netflix-techblog/towards-true-continuous-integration-distributed-repositories-and-dependencies-2a2e3108c051)
6. [Continuous Integration and Deployment with Bazel at Dropbox](https://blogs.dropbox.com/tech/2019/12/continuous-integration-and-deployment-with-bazel/)
7. [Continuous Deployments at BuzzFeed](https://tech.buzzfeed.com/continuous-deployments-at-buzzfeed-d171f76c1ac4)
8. [Screwdriver: Continuous Delivery Build System for Dynamic Infrastructure at Yahoo](https://yahooeng.tumblr.com/post/155765242061/open-sourcing-screwdriver-yahoos-continuous)
9. [CI/CD at Betterment](https://www.betterment.com/resources/ci-cd-shortening-the-feedback-loop/)
10. [CI/CD at Brainly](https://medium.com/engineering-brainly/ci-cd-at-scale-fdfb0f49e031)
11. [Scaling iOS CI with Anka at Shopify](https://engineering.shopify.com/blogs/engineering/scaling-ios-ci-with-anka)
12. [Scaling Jira Server at Yelp](https://engineeringblog.yelp.com/2019/04/Scaling-Jira-Server-Administration-For-The-Enterprise.html)
13. [Auto-scaling CI/CD cluster at Flexport](https://flexport.engineering/how-flexport-halved-testing-costs-with-an-auto-scaling-ci-cd-cluster-8304297222f)