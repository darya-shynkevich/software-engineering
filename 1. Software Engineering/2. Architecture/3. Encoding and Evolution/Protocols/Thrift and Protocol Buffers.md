Both Thrift and Protocol Buffers require a schema for any data that is encoded. To encode the data in Thrift, you would describe the schema in the Thrift interface definition language (IDL) like this:

```
{
    "userName": "Martin",
    "favoriteNumber": 1337,
    "interests": ["daydreaming", "hacking"]

}
```

```
Thrift

struct Person {  
	1: required string userName,  
	2: optional i64 favoriteNumber, 
	3: optional list<string> interests
}
```

```
Protocol Buffers

message Person {
        required string user_name= 1;
        optional int64  favorite_number = 2;
        repeated string interests= 3;

}
```

Thrift and Protocol Buffers each come with a code generation tool that takes a schema definition like the ones shown here, *and produces classes that implement the schema in various programming languages*. Your application code can call this generated code to encode or decode records of the schema.

Thrift has two different binary encoding formats, called ***BinaryProtocol*** and ***CompactProtocol***.
![Pasted image 20230604145548](../../../../_Attachments/Pasted%20image%2020230604145548.png)

Each field has a type annotation (to indicate whether it is a string, integer, list, etc.) and, where required, a length indication (length of a string, number of items in a list). The strings that appear in the data (“Martin”, “daydreaming”, “hacking”) are also encoded as ASCII (or rather, UTF-8), similar to before. Also there no field names, instead, the encoded data contains field tags, which are numbers (1, 2, and 3). Those are the numbers that appear in the schema definition. Field tags are like aliases for fields — they are a compact way of saying what field we’re talking about, without having to spell out the field name.

The ***Thrift CompactProtocol*** encoding is semantically equivalent to BinaryProtocol, but it packs the same information into only 34 bytes. It does this by packing the field type and tag number into a single byte, and by using variable-length integers. Rather than using a full eight bytes for the number 1337, it is encoded in two bytes, with the top bit of each byte used to indicate whether there are still more bytes to come. This means numbers between –64 and 63 are encoded in one byte, numbers between –8192 and 8191 are encoded in two bytes, etc. Bigger numbers use more bytes.

![Pasted image 20230604150115](../../../../_Attachments/Pasted%20image%2020230604150115.png)

***Protocol Buffers*** (which has only one binary encoding format) packing slightly differently, but is otherwise very similar to Thrift’s CompactProtocol. Protocol Buffers fits the same record in 33 bytes.
![Pasted image 20230604150254](../../../../_Attachments/Pasted%20image%2020230604150254.png)

#### Field tags and schema evolution

You can change the name of a field in the schema, since the encoded data never refers to field names, but you cannot change a field’s tag, since that would make all existing encoded data invalid.

***Forward compatibility**:* you can add new fields to the schema, provided that you give each field a new tag number. If old code (which doesn’t know about the new tag numbers you added) tries to read data written by new code, including a new field with a tag number it doesn’t recognize, it can simply ignore that field. The datatype annotation allows the parser to determine how many bytes it needs to skip. This maintains forward compatibility: old code can read records that were written by new code.

***Backward compatibility**:* As long as each field has a unique tag number, new code can always read old data, because the tag numbers still have the same meaning. The only detail is that if you add a new field, you cannot make it required. If you were to add a field and make it required, that check would fail if new code read data written by old code, because the old code will not have written the new field that you added. Therefore, to maintain backward compatibility, every field you add after the initial deployment of the schema must be optional or have a default value.

Removing a field is just like adding a field, with backward and forward compatibility concerns reversed. That means you can only remove a field that is optional (a required field can never be removed), and you can never use the same tag number again (because you may still have data written somewhere that includes the old tag number, and that field must be ignored by new code).

#### Datatypes and schema evolution

What about changing the datatype of a field? That may be possible — check the documentation for details—but there is a risk that values will lose precision or get truncated.

Protocol Buffers uses a repeated marker for fields instead of an array datatype, allowing for easy changes from single-valued to multi-valued fields. Thrift has a dedicated list datatype that supports nested lists.

