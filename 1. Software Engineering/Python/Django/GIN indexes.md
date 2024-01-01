#### **Indexing a JSON Value with B-trees:**

![[Pasted image 20231130235521.png]]

#### How to add GIN in Django:

![[Pasted image 20231130235748.png]]
Then the query below is superfast *(The @> operator checks if a jsonb column contains the given JSON path or values)*

```SQL
SELECT COUNT(*) AS "__count" 
FROM "analytics_event" 
WHERE "analytics_event"."data" @> '{"latitude": 44.523064}';
```

In Django ORM:

```Python

Event.objects.filter(data__contains={ "latitude": 44.523064 }).count()
																	
```

