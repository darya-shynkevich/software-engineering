
### Use get_object_or_404() for Single Objects

1. Only use it in views.  
2. Don’t use it in helper functions, forms, model methods or anything that is not a view or directly view related.

### Be Careful With Queries That Might Throw Exceptions

#### 1. ObjectDoesNotExist vs. DoesNotExist

`ObjectDoesNotExist` can be applied to any model object, whereas `DoesNotExist` is for a specific model.

![Pasted image 20231126221839](Pasted%20image%2020231126221839.png)

#### 2. When You Just Want One Object but Get Three Back

If it’s possible that your query may return more than one object, check for a `MultipleObjectsReturned` exception. Then in the except clause, you can do whatever makes sense, e.g. raise a special exception or log the error.

![Pasted image 20231126221931](Pasted%20image%2020231126221931.png)

# [Query Expressions](Query%20Expressions.md)

# Add Indexes as Needed

When to consider adding indexes:
1. The index would be used frequently, as in 10–25% of all queries.
2. There is real data, or something that approximates real data, so we can analyse the results of indexing.
3. We can run tests to determine if indexing generates an improvement in results.

> When using PostgreSQL, pg_stat_activity tells us what indexes are actually being used.

#### TIP: Class-Based Model Indexes

Django provides the `django.db.models.indexes` module, the `Index` class, and the `Meta.indexes` option. These make it easy to create all sorts of database indexes: just subclass Index, add it to `Meta.indexes`, and you’re done! 

`django.contrib.postgres.indexes` currently includes BrinIndex and GinIndex, but you can imagine HashIndex, GistIndex, SpGistIndex, and more.

# Transactions

The default behaviour of the ORM is to autocommit every query when it is called. In the case of data modification, this means that every time a `.create()` or `.update()` is called, it immediately modifies data in the SQL database.

The way to resolve the risk of database corruption (if we need to execute two operations together) is through the use of database transactions. ***A database transaction is where two or more database updates are contained in a single unit of work.*** If a single update fails, all the updates in the transaction are rolled back. To make this work, a database transaction, by definition, must be atomic, consistent, isolated and durable. Database practitioners often refer to these properties of database transactions using the acronym [ACID](ACID).

#### Wrapping Each HTTP Request in a Transaction

```Python
DATABASES = {
   'default': {

		# ...
		
		'ATOMIC_REQUESTS': True, 
	},
}
```

By setting it to True as shown above, all requests are wrapped in transactions, including those that only read data.

The advantage of this approach is safety: ***all database queries in views are protected***, the disadvantage is ***performance can suffer***. We can’t tell you just how much this will affect performance, as it depends on individual database design and how well various database engines handle locking.

Be careful with `django.http.StreamingHttpResponse` !!!! 

#### Explicit Transaction Declaration

Explicit transaction declaration is one way to ***increase site performance***. In other words, specifying which views and business logic are wrapped in transactions and which are not. The downside to this approach is that it ***increases development time***.

1. Database operations that do not modify the database should not be wrapped in transactions.
2. Database operations that modify the database should be wrapped in a transaction.
3. Special cases including database modifications that require database reads and performance considerations can affect the previous two guidelines.

![Pasted image 20231126231315](Pasted%20image%2020231126231315.png)

> **TIP: Never Wrap Individual ORM Method Calls**: Django’s ORM actually relies on transactions internally to ensure consistency of data. For instance, if an update affects multiple tables because of concrete inheritance, Django has that wrapped up in transactions. Therefore, it is never useful to wrap an individual ORM method [.create(), .update(), .delete()] call in a transaction. Instead, use explicit transactions when you are calling several ORM methods in a view, function, or method.

#### Transactions in MySQL

If the database being used is MySQL, transactions may not be supported depending on your choice of table type such as InnoDB or MyISAM. If transactions are not supported, Django will always function in autocommit mode, regardless of ATOMIC_REQUESTS or code written to support transactions.

