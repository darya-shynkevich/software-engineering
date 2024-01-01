
# Raw SQL expressions

Sometimes database expressions can’t easily express a complex `WHERE` clause. In these edge cases, use the `RawSQL` expression. For example:

```Python
from django.db.models.expressions import RawSQL
queryset.annotate(val=RawSQL("select col from sometable where othercol = %s", (param,)))
```

These extra lookups may not be portable to different database engines (because you’re explicitly writing SQL code) and violate the DRY principle, so you should avoid them if possible.

> `RawSQL` expressions can also be used as the target of `__in` filters:

```Python
queryset.filter(id__in=RawSQL("select id from sometable where col = %s", (param,)))
```

Whenever we write raw SQL we lose elements of security and reusability. Also, in the rare event that the data has to be migrated from one database to another, any database-specific features that you use in your SQL queries will complicate the migration.

# `ExpressionWrapper()` expressions

`ExpressionWrapper` surrounds another expression and provides access to properties, such as `output_field`, that may not be available on other expressions. `ExpressionWrapper` is necessary when using arithmetic on `F()` expressions with different types as described below.

# `F()`expression

An `F()` object represents the value of a model field, transformed value of a model field, or annotated column. It makes it ***possible to refer to model field values and perform database operations using them without actually having to pull them out of the database into Python memory.***

Instead, Django uses the `F()` object to generate an SQL expression that describes the required operation at the database level.

```Python
reporter = Reporters.objects.filter(name='Tintin')
reporter.update(stories_filed=F('stories_filed') + 1)
```

We can also use [`update()`](https://docs.djangoproject.com/en/3.2/ref/models/querysets/#django.db.models.query.QuerySet.update "django.db.models.query.QuerySet.update") to increment the field value on multiple objects - *which could be very much faster than pulling them all into Python from the database, looping over them, incrementing the field value of each one, and saving each one back to the database:*

```Python
Reporter.objects.all().update(stories_filed=F('stories_filed') + 1)
```

This will be translated to a query like:
![Pasted image 20231203013220](../../../_Attachments/Pasted%20image%2020231203013220.png)

`F()` therefore can offer performance advantages by:
- getting the database, rather than Python, to do work
- reducing the number of queries some operations require

#### Avoiding race conditions using `F()`

Another useful benefit of `F()` is that having the database - rather than Python - update a field’s value avoids a _race condition_.

```Python
reporter = Reporters.objects.get(name='Tintin')
reporter.stories_filed += 1
reporter.save()
```

If two Python threads execute the code in the first example above, one thread could retrieve, increment, and save a field’s value after the other has retrieved it from the database. The value that the second thread saves will be based on the original value; the work of the first thread will be lost.

If the database is responsible for updating the field, the process is more robust: it will only ever update the field based on the value of the field in the database when the [`save()`](https://docs.djangoproject.com/en/3.2/ref/models/instances/#django.db.models.Model.save "django.db.models.Model.save") or `update()` is executed, rather than based on its value when the instance was retrieved.

#### `F()` assignments persist after `Model.save()`

> `F()` objects assigned to model fields persist after saving the model instance and will be applied on each [`save()`](https://docs.djangoproject.com/en/3.2/ref/models/instances/#django.db.models.Model.save "django.db.models.Model.save")

```Python
reporter = Reporters.objects.get(name='Tintin')
reporter.stories_filed = F('stories_filed') + 1
reporter.save()

reporter.name = 'Tintin Jr.'
reporter.save()
```

`stories_filed` will be updated twice in this case. If it’s initially `1`, the final value will be `3`. This persistence can be avoided by reloading the model object after saving it, for example, by using [`refresh_from_db()`](https://docs.djangoproject.com/en/3.2/ref/models/instances/#django.db.models.Model.refresh_from_db "django.db.models.Model.refresh_from_db").

#### Using `F()` with annotations

```Python
from django.db.models import DateTimeField, ExpressionWrapper, F

Ticket.objects.annotate(
    expires=ExpressionWrapper(
        F('active_at') + F('duration'), output_field=DateTimeField()
        )
    )
```

When referencing relational fields such as `ForeignKey`, `F()` returns the primary key value rather than a model instance:

```Python
>> car = Company.objects.annotate(built_by=F('manufacturer'))[0]
>> car.manufacturer
<Manufacturer: Toyota>
>> car.built_by
3
```

#### Using `F()` to sort null values

```Python
from django.db.models import F
Company.objects.order_by(F('last_contacted').desc(nulls_last=True))
```

# `Func()` expressions

`Func()` expressions are the base type of all expressions that involve database functions like `COALESCE` and `LOWER`, or aggregates like `SUM`. They can be used directly:

```Python
from django.db.models import F, Func

queryset.annotate(field_lower=Func(F('field'), function='LOWER'))
```

Consider this problem: you have a table in PostgreSQL with millions of rows. One of the columns in the table stores `jsonb` data that, for some rows, has a "count" key whose value is an integer. Other rows, however, lack the "count" key. You want to write a query with the ORM that either increments the current "count" value by one or initialises it to one.

This suffers all the same problems that were present when we tried to increment without an `F()` expression, but the solution is not so easy. An `F()` expression can’t reach into a jsonb column to refer to keys inside, so we can’t just write `.update(data__count=F('data__count') + 1)`.

```Python
class JsonbFieldIncrementer(Func): (1)
	"""Set or increment a property of a JSONB column.""" 
	function = "jsonb_set" 
	arity = 3 
	
	def __init__(
		self, 
		json_column, 
		property_name, 
		increment_by, 
		**extra
	): 
	
	property_expression = Value("{{{}}}".format(property_name)) (2)
	
	set_or_increment_expression = RawSQL( (3)
		(
			"(COALESCE({}->>'{}','0')::int + %s)"
			"::text::jsonb".format(son_column, property_name) (4)
		), 
		(increment_by,)
	) 
	
	super().__init__(
		json_column, 
		property_expression, 
		set_or_increment_expression, 
		**extra
	)

def increment_all_event_counts_with_func(): 
	"""Increment all event counts.""" 
	incr_by_one = JsonbFieldIncrementer('data', 'count', 1)
	Event.objects.all().update(data=incr_by_one).limit(10)
```

1. To write a custom `Func()` expression, sub-class `Func` .
2. This argument is the name of the property to update, which we want to look like `{count}` because the argument type is `jsonpath`. Using `.format() `requires us to escape the curly braces. When you are building an expression from pieces like this example does, you need to wrap individual values in `Value()` expressions.
3. The database function that this class executes is `jsonb_set`. Here, we construct the arguments to the `jsonb_set` call using a `RawSQL()` expression, which itself takes an argument containing the SQL to use and another argument containing values that should be safely interpolated/escaped by the database driver.




# `Aggregate()` expressions

An aggregate expression is a special case of a [Func() expression](https://docs.djangoproject.com/en/3.2/ref/models/expressions/#func-expressions) that informs the query that a `GROUP BY` clause is required. All of the [aggregate functions](https://docs.djangoproject.com/en/3.2/ref/models/querysets/#aggregation-functions), like `Sum()` and `Count()`, inherit from `Aggregate()`.

Since `Aggregate`s are expressions and wrap expressions, you can represent some complex computations:

```Python
from django.db.models import Count

Company.objects.annotate(
    managers_required=(Count('num_employees') / 4) + Count('num_managers'))
```

# `Value()` expressions

A `Value()` object represents the smallest possible component of an expression: a simple value. When you need to represent the value of an integer, boolean, or string within an expression, you can wrap that value within a `Value()`.

# `Subquery()` expressions

For example, to annotate each post with the email address of the author of the newest comment on that post:

```Python
from django.db.models import OuterRef, Subquery
newest = Comment.objects.filter(post=OuterRef('pk')).order_by('-created_at')
Post.objects.annotate(newest_commenter_email=Subquery(newest.values('email')[:1]))
```

On PostgreSQL, the SQL looks like:

```SQL
SELECT "post"."id", (
    SELECT U0."email"
    FROM "comment" U0
    WHERE U0."post_id" = ("post"."id")
    ORDER BY U0."created_at" DESC LIMIT 1
) AS "newest_commenter_email" FROM "post"
```

## Referencing columns from the outer queryset

Use `OuterRef` when a queryset in a `Subquery` needs to refer to a field from the outer query or its transform. It acts like an [`F`](https://docs.djangoproject.com/en/3.2/ref/models/expressions/#django.db.models.F "django.db.models.F") expression except that the check to see if it refers to a valid field isn’t made until the outer queryset is resolved.

```Python
def get_clients(self):  
    return Client.objects.annotate(  
        has_active_partner=Exists(  
            Partner.objects.filter(  
                is_active=True,  
                client_id=OuterRef('id'),  
            ),  
        ),  
    )
```

Instances of `OuterRef` may be used in conjunction with nested instances of `Subquery` to refer to a containing queryset that isn’t the immediate parent. 

For example, this queryset would need to be within a nested pair of `Subquery` instances to resolve correctly:

```Python
Book.objects.filter(author=OuterRef(OuterRef('pk')))
```

## `Exists()` subqueries

`Exists` is a `Subquery` subclass that uses an SQL `EXISTS` statement. In many cases it will perform better than a subquery since the database is able to stop evaluation of the subquery when a first matching row is found.

For example, to annotate each post with whether or not it has a comment from within the last day:

```Python
from django.db.models import Exists, OuterRef
from datetime import timedelta
from django.utils import timezone

one_day_ago = timezone.now() - timedelta(days=1)
recent_comments = Comment.objects.filter(
...   post=OuterRef('pk'),
...   created_at__gte=one_day_ago,
... )
Post.objects.annotate(recent_comment=Exists(recent_comments))
```

On PostgreSQL, the SQL looks like:

```SQL
SELECT "post"."id", "post"."published_at", EXISTS(
    SELECT (1) as "a"
    FROM "comment" U0
    WHERE (
        U0."created_at" >= YYYY-MM-DD HH:MM:SS AND
        U0."post_id" = "post"."id"
    )
    LIMIT 1
) AS "recent_comment" FROM "post"
```

> You can query using `NOT EXISTS` with `~Exists()`.







