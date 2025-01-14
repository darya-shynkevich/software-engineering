#### [`QuerySet`s are lazy](!https://docs.djangoproject.com/en/3.2/topics/db/queries/#querysets-are-lazy)

`QuerySets` are lazy – the act of creating a [`QuerySet`](https://docs.djangoproject.com/en/3.2/ref/models/querysets/#django.db.models.query.QuerySet "django.db.models.query.QuerySet") doesn’t involve any database activity.

In general, the results of a [`QuerySet`](https://docs.djangoproject.com/en/3.2/ref/models/querysets/#django.db.models.query.QuerySet "django.db.models.query.QuerySet") aren’t fetched from the database until you “ask” for them. When you do, the [`QuerySet`](https://docs.djangoproject.com/en/3.2/ref/models/querysets/#django.db.models.query.QuerySet "django.db.models.query.QuerySet") is _evaluated_ by accessing the database.

#### When [QuerySets are evaluated](!https://docs.djangoproject.com/en/3.2/ref/models/querysets/#when-querysets-are-evaluated)

1. Iteration
2. Slicing
3. `repr()`
4. `len()`
5. `list()`
6. `bool()` + `or` + `and` + `if`

#### How [QuerySets are cached](!https://docs.djangoproject.com/en/3.2/topics/db/queries/#caching-and-querysets):

Querysets do not always cache their results. When evaluating only _part_ of the queryset, the cache is checked, but if it is not populated then the items returned by the subsequent query are not cached. Specifically, this means that [limiting the queryset](https://docs.djangoproject.com/en/3.2/topics/db/queries/#limiting-querysets) using an array slice or an index will not populate the cache.

```Python
>>> queryset = Entry.objects.all()
>>> print(queryset[5]) # Queries the database
>>> print(queryset[5]) # Queries the database again
```

However, if the entire queryset has already been evaluated, the cache will be checked instead:

```Python
>>> queryset = Entry.objects.all()
>>> [entry for entry in queryset] # Queries the database
>>> print(queryset[5]) # Uses cache
>>> print(queryset[5]) # Uses cache
```

#### EXPLAIN and **EXPLAIN ANALYZE**

Query Plan - is a visualisation of work that the database expects to do for a given query. In the case of PostgreSQL, this plan is generated by the query planner component, which bases its decisions on statistical data about the database.

![Pasted image 20231130224046](Pasted%20image%2020231130224046.png)

> `Queryset.explain()`

**EXPLAIN** reports the query plan, including the expected cost. **EXPLAIN ANALYZE** is a PostgreSQL-specific command that runs the query and reports the plan alongside the actual execution time.

![Pasted image 20231130224132](Pasted%20image%2020231130224132.png)

(2) "Seq Scan" is a sequential scan of the entire table, in contrast to an index scan or bitmap index scan that uses an available index. 
(3) You read a query plan inside-out. This Filter node represents the WHERE clause in the query.

*The cost listed on the top-most node of the query plan is the cost for the entire query. The number reads like this: cost=.. . The number represents the estimated disk page fetches and CPU time needed to run the query. This query has a high cost, which equates to 2,758,484 rows accessed and an over 2 second execution time.*

## [Indexes](1.%20Software%20Engineering/3.%20Database/OTLP/SQL/2.%20Indexes/_Base.md)

How to create an index in Django:
![Pasted image 20231130233708](Pasted%20image%2020231130233708.png)

#### Adding Indexes in Production

The default setting in PostgreSQL is to lock the table you are indexing against writes during the indexing process. On the other hand, MySQL with the InnoDB engine doesn’t lock by default.

# Query performance improvements

1. Going faster by making fewer queries 
2. Requesting only the data you need 
3. Reducing object overhead by working with native Python types and Saving memory when iterating over large querysets 
4. Updating records more efficiently 
5. Moving work to the database 
6. Avoiding unbounded queries by paginating with Paginator 
7. Speeding up pagination by using "keyset" pagination 
8. Caching slow query results within the database with materialized views

### 1. The N+1 Problem and select_related

`select_related` obtains all data at one time through multi-table join Association query and improves performance by reducing the number of database queries. It uses [4. JOIN statement](4.%20JOIN%20statement.md)s of SQL to optimize and improve performance by reducing the number of SQL queries. *(FK and One2One)*

`prefetch_related` works by fetching the related data in a separate query after the initial query has been executed. This is to solve the problem in the SQL query through a JOIN statement. However, for many-to-many relationships, it is not wise to use SQL statements to solve them, because the tables obtained by JOIN will be very long, which will lead to an increase in running time and memory occupation of SQL statements. The solution to prefetch_related() is to query each table separately and then use Python to handle their relationship! *(m2m, m2one (reversed FK))*

```Python

Province.objects.prefetch_related('city_set').get(name__iexact="Hubei Province")
```

```SQL

SELECT `Optimize_province`.`id`, `Optimize_province`.`name`
FROM `Optimize_province`
WHERE `Optimize_province`.`name` LIKE 'Hubei Province';
```
```SQL
SELECT `Optimize_city`.`id`, `Optimize_city`.`name`, `Optimize_city`.`province_id`
FROM `Optimize_city`
WHERE `Optimize_city`.`province_id` IN (1);
```

As we can see, prefetch is implemented using the `IN` statement. In this way, when there are too many objects in QuerySet, performance problems may arise depending on the characteristics of the database.

#### Outer Join vs. Inner Join

Django uses a process to determine what types of joins to use when building a SQL query known as "[Join Promotion](Join%20Promotion.md)"

### 2. Limiting the Data Returned by Queries

When you use a queryset, Django retrieves data for all of the model’s fields by default.

If your model has a field that contains a large amount of data, e.g. a BinaryField or just a large TextField , including that field in queries that don’t explicitly need it can make the query slow and consume memory unnecessarily.
#### Use only() to Return Only the Columns You Want

The `only()` method on QuerySet allows you to specify which columns of a table Django should request in a query. The other fields on the model instances returned by the QuerySet ***become lazy***, meaning that if you access one of these fields, Django will make an additional SQL query to retrieve its value. Meanwhile, the fields you specified in `only()`  will be present directly on the model instance.

#### Using only() with Relations

![Pasted image 20231202211248](Pasted%20image%2020231202211248.png)

1. We select the first Event object but only retrieve its related user. 
2. Because we did not include "name" in the query, accessing event.name requires an additional query. 
3. Accessing "user" does not require an additional query.

#### Use defer() to Specify the Columns You Do Not Want

The `defer()` method on QuerySet is the opposite of `only()` : while `only() `specifies the columns that should be available on objects in the queryset and all other columns are made "lazy," `defer()` specifies the columns that should be lazily-accessible and all others are available on objects.

***Put here attribute with is expensive and you do not know if sb will use it.***

### 3. Reducing ORM Memory Overhead

Django has a couple of useful tools for memory problem: 
1. `values()` - good for reading data without instantiating models
2. `iterator()` - useful for working with large querysets without loading the entire result set into memory at one time.

### 4. Faster Updates

Use `update()` when: 
1. You are updating all objects in a queryset to the same set of values. 
2. The update does not need `pre_save` or `post_save` signal handlers to run, the `auto_now` option, or any custom `save()` methods.

**`bulk_update()`** gives you more control over the values of fields to update, while still having performance benefits over calling `save()` for every model you want to update. Because `bulk_update()` takes model instances, you can change the same field on multiple models to different values, unlike with `update()`.

![Pasted image 20231203012451](Pasted%20image%2020231203012451.png)
1. The first parameter is a list of objects, and the second is an iterable of the fields to update.

Use bulk_update() when: 
1. You have enough objects to update that calling `save()` on each would generate too many queries and/or take too long. Ideally, performance monitoring led you to optimise this. 
2. You are updating objects in a queryset to different values (use `update()` if you are updating the objects to the same values or require more complicated logic to control which objects to update). 
3. The update does **not** need `pre_save` or `post_save` signal handlers to run, the `auto_now` option, or any custom `save()` methods.

If you are updating a large number of objects. use the **`batch_size`** parameter *to split the work into multiple queries.*

> Working with large numbers of model instances consumes memory that you might be able to avoid. 

### 5.  Moving Work to the Database
#### 5.1 [Query Expressions](Query%20Expressions.md) 

Databases are usually blobs of optimized C or C++, so they’re going to be faster at CPU-bound work than Python. The data also lives in the database, so running calculations there incurs less latency and serialization costs — the work required to transfer data between machines and in different formats

For these reasons, when optimizing views that use Python to calculate, reach for annotations. If, instead of adding information to the results of a query, you want to reduce the results of the query to a summary statistic, use aggregations. We’ll discuss both approaches in the next sections.
#### 5.2 When You Really Do Need to Calculate in Python

For example, what if part of the data you need is in your database and the other part is in a web service? You’ll need to query the web service and do the computation in Python.

If you use PostgreSQL and frequently query data in remote services, consider Postgres’s "[Foreign Data Wrappers](Foreign%20Data%20Wrappers)" feature

#### 5.3 Annotations

```Python
completed_tasks = Subquery( 
	(
		TaskStatus.objects.filter(
			task__goal=OuterRef('pk'), 
			status=TaskStatus.DONE 
		)
		.values('task__goal')
		.annotate(count=Count('pk'))
		.values('count'),
	)
	output_field=IntegerField()
)

goals = (
	Goal.objects.all()
	.annotate(completed_tasks=Coalesce(completed_tasks, 0))
	.order_by('-completed_tasks')
)[:10]
```

#### 5.4 Aggregations

```Python
average_completions = goals.aggregate(Avg('completed_tasks'))
```

Instead of adding a number to each item in the queryset, an aggregation produces a single number from the queryset. In this case, we average the number of completed tasks previously calculated by an annotation.

#### 5.5 [Materialised Views](Materialised%20Views.md)

![Pasted image 20231203165627](Pasted%20image%2020231203165627.png)
1. Specifying `managed = False` means that Django won’t create any database operations to modify the database for this model.

Then you will need to write a **custom migration** 

![Pasted image 20231203165758](Pasted%20image%2020231203165758.png)
1. This creates a new view called `goals_goalsummary` whose content is the result of internal SELECT query.

Also we will need a **management command**

1. Ad-hoc management command run by a human operator (irregular/manual refresh interval) 
2. Cron job that runs a management command on a schedule (regular interval, and you use cron) 
3. Scheduled Celery task that executes the SQL query (regular interval, and you use Celery)

```Python
class Command(BaseCommand): 
	help = 'Refresh Goal summaries' 
	def handle(self, *args, **options): 
		with transaction.atomic(): 
			with connection.cursor() as cursor:
				cursor.execute(
				"REFRESH MATERIALIZED VIEW CONCURRENTLY goals_summaries"
				)
```

1. We execute in a transaction to make sure we write consistent data to the view.
2. The `CONCURRENTLY` keyword tells Postgres to allow other clients to read from this view while it refreshes.

*When you represent a materialised view with a model, you can query it like any other model.*

### 6. Bounding Queries with Pagination

![Pasted image 20231203170608](Pasted%20image%2020231203170608.png)
1. When using offset pagination, the results must have a stable order for pages to be consistent.

> In the previous example, we passed in a QuerySet object to Paginator() , but it’s worth pointing out that Paginator can paginate any sliceable object with a count() or _len_() method

#### 6.1 Problems With Offset Pagination

Offset-based pagination like that provided by Paginator has two flaws: results can be ***inconsistent***, and when there are many pages of results, ***the farther into the result set the client paginates, the longer the queries take.***

Inconsistency can happen because a page number like 5 is only translated into a set of records to return at the time of a client’s query. Deletions and insertions might have occurred between the client’s last query — for e.g. page 4 — and the time of the current query, so records that were on page 4 moments ago now appear again on page 5.

#### 6.2 Keyset Pagination

Keyset pagination is an alternative to offset pagination that solves some of its problems. 

A common user experience built with keyset pagination is "endless scroll," in which scrolling down in a web page automatically loads more content. 

```Python
Customer.objects.all().order_by('created_date', 'pk')
```

`CursorPaginator` in DRF

### 7. Cache Queries With Memcached or Redis

[Upstream Caches](Upstream%20Caches) such as [Varnish](Varnish) are very useful. They run in front of your web server and speed up web page or content serving significantly.

[Content Delivery Networks](Content%20Delivery%20Networks) (CDNs) like Fastly, Akamai, and Amazon Cloudfront serve static media such as images, video, CSS, and JavaScript files. They usually have servers all over the world, which serve out your static content from the nearest location. Using a CDN rather than serving static content from your application servers can speed up your projects.