uses a schema to specify the structure of the data being encoded. It has two schema languages: one (Avro IDL) intended for human editing, and one (based on JSON) that is more easily machine-readable.

Our example schema, written in Avro IDL, might look like this:

```
record Person {  
	string userName;  
	union { null, long } favoriteNumber = null; 
	array<string> interests;
}
```

The equivalent JSON representation of that schema is as follows:

```
{
        "type": "record",
        "name": "Person",
        "fields": [
			{"name": "userName", "type": "string"},  
			{"name": "favoriteNumber", "type": ["null", "long"], "default": null}, 
			{"name": "interests", "type": {"type": "array", "items": "string"}}
] }
```

There is nothing to identify fields or their datatypes. The encoding simply consists of values concatenated together (no tag numbers in the schema).
![[Pasted image 20230604151743.png]]
To parse the binary data, you go through the fields in the order that they appear in the schema and use the schema to tell you the datatype of each field. This means that the binary data can only be decoded correctly if the code reading the data is using the exact same schema as the code that wrote the data. Any mismatch in the schema between the reader and the writer would mean incorrectly decoded data.

#### The writer’s schema and the reader’s schema

When an application wants to encode some data (to write it to a file or database, to send it over the network, etc.), it encodes the data using *whatever version of the schema it knows about*—for example, that *schema may be compiled into the application*. This is known as the writer’s schema.

When an application wants to decode some data (read it from a file or database, receive it from the network, etc.), it is expecting the data to be in some schema, which is known as the *reader’s schema*. That is the schema the application code is relying on —code may have been generated from that schema during the application’s build process.

The key idea with Avro is that the writer’s schema and the ***reader’s schema don’t have to be the same***  — they only need to be compatible.

For example, it’s no problem if the writer’s schema and the reader’s schema have their fields in a different order, because the schema resolution matches up the fields by field name. If the code reading the data encounters a field that appears in the writer’s schema but not in the reader’s schema, it is ignored. If the code reading the data expects some field, but the writer’s schema does not contain a field of that name, it is filled in with a default value declared in the reader’s schema.
![[Pasted image 20230604152103.png]]

#### Schema evolution rules

To maintain compatibility, you may only add or remove a field that has a default value. If you were to add a field that has no default value, new readers wouldn’t be able to read data written by old writers, so you would break backward compatibility. If you were to remove a field that has no default value, old readers wouldn’t be able to read data written by new writers, so you would break forward compatibility.

Consequently, Avro doesn’t have optional and required markers in the same way as Protocol Buffers and Thrift do (it has union types and default values instead).

#### The context in which Avro is being used:

1. ***Large file with lots of records:*** *the writer of that file can just include the writer’s schema once at the beginning of the file. Avro specifies a file format (object container files) to do this.*
2. ***Database with individually written records**:* *In a database, different records may be written at different points in time using different writer’s schemas — you cannot assume that all the records will have the same schema. The simplest solution is to include a version number at the beginning of every encoded record, and to keep a list of schema versions in your database. A reader can fetch a record, extract the version number, and then fetch the writer’s schema for that version number from the database. Using that writer’s schema, it can decode the rest of the record.* *As the version number, you could use a simple incrementing integer, or you could use a hash of the schema.*
3. ***Sending records over a network connection:*** *When two processes are communicating over a bidirectional network connec‐ tion, they can negotiate the schema version on connection setup and then use that schema for the lifetime of the connection.*




