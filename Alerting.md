
## The levels of monitoring & alerts

![[Pasted image 20231014191613.png]]

### Operational alerts

The first type of alerts are the operation alerts. This category focusses on making sure that everything keeps running smoothly at the service/bare-metal level. These alerts monitor for example how many messages are in a certain queue and what the age of the oldest message is. This gives us a good idea if our processes are healthy.

### Data validation

The previous alert gave an overview of operational load & throughput. What it did not show was if certain values were received and if the amount of data was (more or less) correct. Other metrics that fall under data validation are:

- Did we receive (enough) files for a given date?
- Was the amount of data relatively close to the average amount of ingested rows of previous days?
- Can the incoming files be unzipped/loaded correctly?
- Did we receive the file at the correct timestamp?
- ...

The knowledge is quite superficial, as we're only looking at the **amount** of data and not the semantics of the data itself.

### Business Assumptions

Business assumptions come in all flavours. To give an idea, I'll give a few examples:

- We assume the header of the file always has a certain date format, but we're not sure if this will transfer to newly received files.
- We assume 2 values always add up to a third value. If this is incorrect, our next calculations cascade the faultiness.
- Are the values in a certain row correct according to our initial research?
- Are values within a certain range without too many outliers?
- ...

