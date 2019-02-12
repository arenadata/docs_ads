Overview
=========

Developed by **Arenadata** platform **ADS** has the ability to act as a unified platform for handling all the real-time data feeds a large company might have. To do this we had to think through a fairly broad set of use cases:

+ **ADS** would have to have high-throughput to support high volume event streams such as real-time log aggregation;
+ **ADS** would need to deal gracefully with large data backlogs to be able to support periodic data loads from offline systems;
+ **ADS** also meant the system would have to handle low-latency delivery to handle more traditional messaging use-cases.

**Arenadata** wanted to support partitioned, distributed, real-time processing of these feeds to create new, derived feeds. This motivated our partitioning and consumer model.

Finally in cases where the stream is fed into other data systems for serving, the system would have to be able to guarantee fault-tolerance in the presence of machine failures.

Supporting these uses led **Arenadata** to a design with a number of unique elements, more akin to a database log than a traditional messaging system. 
