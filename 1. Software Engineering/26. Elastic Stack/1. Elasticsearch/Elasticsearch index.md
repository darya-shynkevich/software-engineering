An index on Elasticsearch is a location to store and organise related documents.

> *When a document is indexed into Elasticsearch, it is indexed by the primary shard before being replicated to the replica shard. The indexing request is only acknowledged once the replica shard has been successfully updated, ensuring read consistency across the Elasticsearch cluster.*

Replica shards can respond to search (or read) requests independently of the primary shard.

## Inside an index

Elasticsearch documents are JSON objects that are stored in indices.

When a document is indexed into Elasticsearch, it is stored in the `_source field`. The following additional system fields are also added to each document:
1. The name of the index the document is stored in is indicated by the `_index` field.
2. An index-wide unique identifier for the document is stored in the `_id` field.

### Data types

**Keyword fields** are string values that are used for filtering, sorting, and aggregations rather than full-text search.
Examples:
1. country: Australia
2. http.status_code: 200
Keyword terms must match the value in question. For example, `country: Australia` will return a hit while `country: australia` will not.

**Text fields** are string values that need to be analysed and are used for full-text search use cases.
Examples:
1. author_bio: William Shakespeare was an English playwright, poet, and actor, widely regarded as the greatest writer… •
2. city_description: Melbourne is the coastal capital of the south-eastern Australian state of Victoria. At the city's center is the…

Integer fields are used to store numerical values on Elasticsearch. These field types support metric aggregations such as `min`, `max`, and `avg`, as well as `range filters`. Depending on the size of the numeric value to be used, types such as **integer**, **long**, **double**, and **float** can be used.
Examples:
1. http.response_time_ms: 324 
2. monthly_sales_aud: 40000 
3. population_M: 3.41

**Date fields** in Elasticsearch can be represented in several formats:
1. A long value of milliseconds since the epoch
2. An integer value of seconds since the epoch
3. A formatted string value, such as yyyy-MM-dd HH:mm:ss
Date fields can be configured to accept multiple formats.
Dates in Elasticsearch should be stored in the **UTC time zone**. Kibana will typically map this to the local time zone based on the browser.
Examples:
1. @timestamp: 2020-12-23T03:53:36.431Z
2. event.start: 1610755518293
3. event.end: 1610755554

Valid **IPv4** and **IPv6** values can be stored as `ip` fields in Elasticsearch. IP ranges (in CIDR notation) should be stored as `ip_range` fields.
Mapping `ip` fields allows you to easily search/filter data based on subnet ranges.
Examples:
1. source.ip: 10.53.0.1
2. destination.ip: 2001:0db8:85a3:0000:0000:8a2e:0370:7334
3. source.subnet: 10.53.0.0/16

True or false values should be mapped as **boolean** types on Elasticsearch. Boolean values can be either of the following:
1. JSON true and false values
2. Strings containing true or false
Examples:
1. event.allowed: false
2. user.is_admin: true

Geo-location data (latitude, longitude pairs) can be mapped as a **geo_point** on Elasticsearch. The simplest representation of `geo_point` data in a document is an object containing `lat` and `lon` values.
Use cases:
1. Filtering documents based on their distance from another `geo_point` or location within a bounding box
2. Sorting documents based on their distance from a geo_point
3. Aggregating documents to visualize them on areas in a map
Examples:
1. source.geo_location: { "lat": 41.12, "lon": -71.34}
2. merchant.business_location: [-21.13, 30.10]

The **object** data type can be used to represent an inner object in the primary JSON document that is indexed into Elasticsearch. The object data type is set by default wherever a field contains subfields in an indexed document.
Keys in an objects **are flattened** by default by Elasticsearch.
Original document:
```json
{
   "event":{
      "type":"http",
      "status":"complete"
   },
   "http":{
      "response":{
         "code":500
      },
      "version":"1.1"
   },
   "@timestamp":"2020-12-23T03:53:36.431Z"
}
```
Flattened:
```json
{
   "event.type":"http",
   "event.status":"complete",
   "http.response.code":500,
   "http.version":"1.1",
   "@timestamp":"2020-12-23T03:53:36.431Z"
}
```

**Array:** More than one value for a field can be stored in Elasticsearch as an **array** (or list of values). Arrays do not need to be explicitly defined in the mapping (and there is no data type for an array); a field with any mapped data type can hold one or more values if required.
An array can only hold a singular data type. A field mapped as a string can only accept arrays with all string values. 
If an array contains elements that have different data types, **the array can be indexed if the data type can be converted (coerced) into the mapped type.** ([coercion settings](https://www.elastic.co/guide/en/elasticsearch/reference/8.0/coerce.html))
Original document:
```json
{
   "product":"Toyota Corolla",
   "stores":[
      {
         "suburb":"Carlton",
         "capacity":150
      },
      {
         "suburb":"Newtown",
         "capacity":20
      },
      {
         "suburb":"Fitzroy",
         "capacity":40
      }
   ]
}
```
Flattened:
```json
{
   "product":"Toyota Corolla",
   "stores.suburb":[
      "Carlton",
      "Newtown",
      "Fitzroy"
   ],
   "stores.capacity":[
      150,
      20,
      40
   ]
}
```
NOTE: queries that rely on preserving the relationship between Carlton and its capacity of 150 cars can no longer be run successfully. Use nested type.

The **nested** type allows you to index an array of objects, where the context of each object is preserved for querying. Standard query functions will not work in nested fields because of the internal representation of these fields => should use **nested query syntax**. 
NOTE: visualising nested objects is not supported in [Kibana](Kibana.md).
NOTE: Nested objects can also impact search performance; documents should be denormalised where performance is necessary.

The **join** data type allows you to create parent/child relationships across documents in an index. 
NOTE: All the related documents must exist on the same shard within an index.
NOTE: You can use the `has_parent` and `has_child` queries to run joined searches on your data.

## Index templates

An index template on Elasticsearch is a blueprint for index settings and mappings. It is common to distribute a data source across multiple indices on Elasticsearch.
