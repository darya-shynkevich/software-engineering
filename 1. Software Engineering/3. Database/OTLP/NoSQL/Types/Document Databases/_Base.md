A **document database** is designed to store and retrieve documents in formats like XML, JSON, BSON, and so on. These documents are composed of a hierarchical tree data structure that can include maps, collections, and scalar values. Documents in this type of database may have varying structures and data. MongoDB and Google Cloud Firestore are examples of document databases.

**Use case**: Document databases are ***suitable for unstructured catalog data, like JSON files or other complex structured hierarchical data***. For example, in e-commerce applications, a product has thousands of attributes, which is unfeasible to store in a relational database due to its impact on the reading performance. Here comes the role of a document database, which can efficiently store each attribute in a single file for easy management and faster reading speed. Moreover, it’s also a good option for content management applications, such as blogs and video platforms. An entity required for the application is stored as a single document in such applications.

The following example shows data stored in a JSON document. This data is about a person. Various attributes are stored in the file, including `id`, `name`, `email`, and so on.

![](../../../../../../_Attachments/Pasted%20image%2020240119185716.png)
# References:

1. [Document Databases](https://msdn.microsoft.com/en-us/magazine/hh547103.aspx)
2. [Couchbase Ecosystem at LinkedIn](https://engineering.linkedin.com/blog/2017/12/couchbase-ecosystem-at-linkedin)
3. [SimpleDB at Zendesk](https://medium.com/zendesk-engineering/resurrecting-amazon-simpledb-9404034ec506)
4. [Espresso: Distributed Document Store at LinkedIn](https://engineering.linkedin.com/espresso/introducing-espresso-linkedins-hot-new-distributed-document-store)