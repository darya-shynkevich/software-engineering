**django-model-utils** to handle common patterns like **TimeStampedModel**

Model inheritance in Django is a tricky subject. Django provides three ways to do model inheritance: **abstract base classes**, **multi-table inheritance**, and **proxy models**.

![Pasted image 20231126212135](Pasted%20image%2020231126212135.png)

- If the overlap between models is minimal (e.g. you only have a couple of models that share one or two identically named fields), there might not be a need for model inheritance. Just add the fields to both models.
- If there is enough overlap between models that maintenance of models’ repeated fields cause confusion and inadvertent mistakes, then in most cases the code should be refactored so that the common fields are in an abstract base model.
- Proxy models are an occasionally-useful convenience feature, but they’re very different from the other two model inheritance styles.
- At all costs, everyone should avoid multi-table inheritance, instead use explicit OneToOneFields and ForeignKeys between models so you can control when joins are traversed.

## Abstract base class

```Python
class Model(models.Model):  
    created_at = models.DateTimeField(auto_now_add=True)  
    updated_at = models.DateTimeField(auto_now=True)  
  
    class Meta:  
        abstract = True # Django doesn’t create a core_timestampedmodel table when migrate is run.
```

# Migrations

## Adding Python Functions and Custom SQL to Migrations

docs.djangoproject.com/en/3.2/ref/migration-operations/#runpython
docs.djangoproject.com/en/3.2/ref/migration-operations/#runsql

#### Getting Access to a Custom Model Manager’s Methods

`use_in_migrations = True` to use custom model manager methods

```Python
class MyManager(models.Manager):
    use_in_migrations = True

class MyModel(models.Model):
    objects = MyManager()
```

### Getting Access to a Custom Model Method

Due to how `django.db.migrations` serializes models, there’s ***no way around this limitation***. You simply cannot call any custom methods during a migration

! If you override a model’s save and delete methods, they won’t be called when called by RunPython.

https://docs.djangoproject.com/en/3.2/topics/migrations/#historical-models 

### Use RunPython.noop to Do Nothing

```Python
migrations.RunPython(add_cones, migrations.RunPython.noop)
```

## Deployment and Management of Migrations

1. Always back up your data before running a migration.
2. Before deployment, check that you can rollback migrations.
3. If a project has tables with millions of rows in them, do extensive tests against data of that size on staging servers before running a migration on a production server. 
4. If you are using MySQL:
    1. You absolutely positively must back up the database before any schema change. MySQL lacks transaction support around schema changes, hence rollbacks are impossible.
    2. If you can, put the project in read-only mode before executing the change.
    3. If not careful, schema changes on heavily populated tables can take a long time. Not seconds or minutes, but hours.

# Django Model Design

### 1. Start Normalised

### 2. Cache Before Denormalising

### 3. Denormalise Only if Absolutely Needed

It’s a tricky process that risks adding complexity to your project and dramatically raises the risk of losing data.
### 4. When to Use Null and Blank

![Pasted image 20231126213907](Pasted%20image%2020231126213907.png)

### 5. When to Use BinaryField

No filters, excludes, or other SQL actions on the field

Use cases:
1. MessagePack-formatted content.
2. Raw sensor data.
3. Compressed data e.g. the type of data Sentry stores as a BLOB, but is required to base64-encode due to legacy issues.

Binary data can come in huge chunks, which can slow down databases. If this occurs and becomes a bottleneck, the solution might be to save the binary data in a file and reference it with a FileField.

Why not to store a file in DB:
1. read/write to a DB is always slower than a filesystem
2. your DB backups grow to be huge and more time consuming
3. access to the files now requires going through your app (Django) and DB layers (can't use nginx or another lightweight web server to serve images up)

Why not to store ephemeral data:
(usage statistics, metrics, GPS locations, session data anything that is only useful to you for a short period of time or frequently changes)
1. Can affect the performance of the main DB due to constant update/delete

Why not to store logs:
1. Can affect the performance of the main DB due to constant insert (turn up your logging to a verbose or debug level and watch your production database catch on fire!)

# 6. Try to Avoid Using Generic Relations

The idea of generic relations is that we are binding one table to another by way of an unconstrained foreign key (GenericForeignKey).

1. Reduction in speed of queries due to lack of indexing between models.
2. Danger of data corruption as a table can refer to another against a non-existent record.

The upside of this lack of constraints is that generic relations *make it easier to build apps for things that need to interact with numerous model types* we might have created. Specifically things like favourites, ratings, voting, messages, and tagging apps. 

1. Try to avoid generic relations and GenericForeignKey.
2. If you think you need generic relations, see if the problem can be solved through better model design or the new PostgreSQL fields.
3. If usage can’t be avoided, try to use an existing third-party app. The isolation a third-party app provides will help keep data cleaner.

## 7.  Make Choices and Sub-Choices Model Constants

```Python
flavor = models.CharField(
        max_length=2,
        choices=FLAVOR_CHOICES
       )
```

## 8. Using Enumeration Types for Choices

```Python
from django.db import models

class IceCreamOrder(models.Model): 

	   class Flavors(models.TextChoices):
           CHOCOLATE = 'ch', 'Chocolate'
           VANILLA = 'vn', 'Vanilla'
           STRAWBERRY = 'st', 'Strawberry'
           CHUNKY_MUNKY = 'cm', 'Chunky Munky'

       flavor = models.CharField(
           max_length=2,
           choices=Flavors.choices
       )
```

Drawbacks:
1. Named groups are not possible with enumeration types. What this means is that if we want to have categories inside our choices, we’ll have to use the older tuple-based approach
2. If we want other types besides str and int, we have to define those ourselves

# The Model `_meta` API

*The reason for this is that before Django 1.8, the model `_meta` API was unofficial and purposely undocumented, as is normal with any API subject to change without notice. The original purpose of `_meta` was simply for Django to store extra info about models for its own use.* ***However, it proved so useful that it is now a documented API.***

1. Get a list of a model’s fields.
2. Get the class of a particular field for a model (or its inheritance chain or other info derived from such).
3. Ensure that how you get this information remains constant across future Django versions.

Examples of these sorts of situations:
1. Building a Django model introspection tool.
2. Building your own custom specialised Django form library.
3. Creating admin-like tools to edit or interact with Django model data.
4. Writing visualisation or analysis libraries, e.g. analyzing info only about fields that start with “foo”.

# Model Managers

Model managers are said to act on the full set of all possible instances of this model class (all the data in the table) to restrict the ones you want to work with. Django provides a default model manager for each model class, but we can define our own.

# Understanding Fat Models

The problem with putting all logic into models is it can cause models to explode in size of code, becoming what is called a ‘god object’. This anti-pattern results in model classes that are hundreds, thousands, even tens of thousands of lines of code. Because of their size and complexity, god objects are hard to understand, hence hard to test and maintain.

If a model starts to become unwieldy in size, we begin isolating code that is prime for reuse across other models, or whose complexity requires better management. The methods, classmethods, and properties are kept, but the logic they contain is moved into ***Model Behaviors*** or ***Stateless Helper Functions***. Let’s cover both techniques in the following subsections:
### Model Behaviors a.k.a Mixins

Model behaviors embrace the idea of composition and encapsulation via the use of mixins. Models inherit logic from abstract models.

### Stateless Helper Functions

By moving logic out of models and into utility functions, it becomes more isolated. This isolation makes it easier to write tests for the logic. The downside is that the functions are stateless, hence all arguments have to be passed.


# References:

1. [Three things you should never put in your database](!https://www.revsys.com/tidbits/three-things-you-should-never-put-your-database/)
2. [Avoid Django's GenericForeignKey](!https://lukeplant.me.uk/blog/posts/avoid-django-genericforeignkey/)
3. [Django Model Behaviors](!https://blog.kevinastone.com/django-model-behaviors)