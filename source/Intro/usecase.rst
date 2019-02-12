Recommendations for use
============================


ADS as a Messaging System
--------------------------

How does ADS's notion of streams compare to a traditional enterprise messaging system?

Messaging traditionally has two models: queuing and publish-subscribe. In a queue, a pool of consumers may read from a server and each record goes to one of them; in publish-subscribe the record is broadcast to all consumers. 

Each of these two models has a strength and a weakness. The strength of queuing is that it allows you to divide up the processing of data over multiple consumer instances, which lets you scale your processing. Unfortunately, queues aren't multi-subscriber—once one process reads the data it's gone. Publish-subscribe allows you broadcast data to multiple processes, but has no way of scaling processing since every message goes to every subscriber.

The consumer group concept in **ADS** generalizes these two concepts. As with a queue the consumer group allows you to divide up processing over a collection of processes (the members of the consumer group). As with publish-subscribe, **ADS** allows you to broadcast messages to multiple consumer groups.

The advantage of ADS's model is that every topic has both these properties—it can scale processing and is also multi-subscriber—there is no need to choose one or the other.

**ADS** has stronger ordering guarantees than a traditional messaging system, too.

A traditional queue retains records in-order on the server, and if multiple consumers consume from the queue then the server hands out records in the order they are stored. However, although the server hands out records in order, the records are delivered asynchronously to consumers, so they may arrive out of order on different consumers. This effectively means the ordering of the records is lost in the presence of parallel consumption. Messaging systems often work around this by having a notion of "exclusive consumer" that allows only one process to consume from a queue, but of course this means that there is no parallelism in processing.

**ADS** does it better. By having a notion of "parallelism -- the partition -- within the topics", **ADS** is able to provide both ordering guarantees and load balancing over a pool of consumer processes. This is achieved by assigning the partitions in the topic to the consumers in the consumer group so that each partition is consumed by exactly one consumer in the group. By doing this we ensure that the consumer is the only reader of that partition and consumes the data in order. Since there are many partitions this still balances the load over many consumer instances. Note however that there cannot be more consumer instances in a consumer group than partitions.


ADS as a Storage System
------------------------

Any message queue that allows publishing messages decoupled from consuming them is effectively acting as a storage system for the in-flight messages. What is different about **ADS** is that it is a very good storage system.

Data written to **ADS** is written to disk and replicated for fault-tolerance. **ADS** allows producers to wait on acknowledgement so that a write isn't considered complete until it is fully replicated and guaranteed to persist even if the server written to fails.

The disk structures **ADS** uses scale well -- **ADS** will perform the same whether you have *50 KB* or *50 TB* of persistent data on the server.

As a result of taking storage seriously and allowing the clients to control their read position, you can think of **ADS** as a kind of special purpose distributed filesystem dedicated to high-performance, low-latency commit log storage, replication, and propagation.


ADS for Stream Processing
-------------------------

It isn't enough to just read, write, and store streams of data, the purpose is to enable real-time processing of streams.

In **ADS** a stream processor is anything that takes continual streams of data from input topics, performs some processing on this input, and produces continual streams of data to output topics.

For example, a retail application might take in input streams of sales and shipments, and output a stream of reorders and price adjustments computed off this data.

It is possible to do simple processing directly using the producer and consumer APIs. However for more complex transformations **ADS** provides a fully integrated Streams API. This allows building applications that do non-trivial processing that compute aggregations off of streams or join streams together. This facility helps solve the hard problems this type of application faces: handling out-of-order data, reprocessing input as code changes, performing stateful computations, etc.

The streams API builds on the core primitives **ADS** provides: it uses the producer and consumer APIs for input, uses **ADS** for stateful storage, and uses the same group mechanism for fault tolerance among the stream processor instances.

