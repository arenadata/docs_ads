Guarantees
===========

At a high-level **ADS** gives the following guarantees:

+ Messages sent by a producer to a particular topic partition will be appended in the order they are sent. That is, if a record *M1* is sent by the same producer as a record *M2*, and *M1* is sent first, then *M1* will have a lower offset than *M2* and appear earlier in the log;
+ A consumer instance sees records in the order they are stored in the log;
+ For a topic with replication factor *N*, we will tolerate up to *N-1* server failures without losing any records committed to the log.

