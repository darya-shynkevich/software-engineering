MapReduce is a programming model for processing large amounts of data in bulk across many machines, popularized by Google. A limited form of MapReduce is supported by some NoSQL datastores, including MongoDB and CouchDB, as a mechanism for performing read-only queries across many documents.

MapReduce is neither a declarative query language nor a fully imperative query API, but somewhere in between: *the logic of the query is expressed with snippets of code, which are called repeatedly by the processing framework*. It is based on the map (also known as collect) and reduce (also known as fold or inject) functions that exist in many functional programming languages.

