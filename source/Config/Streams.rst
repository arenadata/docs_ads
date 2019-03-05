Streams Configs
=========================

Below is the configuration of the Kafka Streams client library.

**application.id** -- An identifier for the stream processing application. Must be unique within the Kafka cluster. It is used as 1) the default client-id prefix, 2) the group-id for membership management, 3) the changelog topic prefix

+ TYPE -- string
+ IMPORTANCE -- high

**bootstrap.servers** -- A list of host/port pairs to use for establishing the initial connection to the Kafka cluster. The client will make use of all servers irrespective of which servers are specified here for bootstrapping—this list only impacts the initial hosts used to discover the full set of servers. This list should be in the form "host1:port1,host2:port2,...". Since these servers are just used for the initial connection to discover the full cluster membership (which may change dynamically), this list need not contain the full set of servers (you may want more than one, though, in case a server is down)

+ TYPE -- list
+ IMPORTANCE -- high

**replication.factor** -- The replication factor for change log topics and repartition topics created by the stream processing application

+ TYPE -- int
+ DEFAULT -- 1
+ IMPORTANCE -- high

**state.dir** -- Directory location for state store

+ TYPE -- string
+ DEFAULT -- /tmp/kafka-streams
+ IMPORTANCE -- high

**cache.max.bytes.buffering** -- Maximum number of memory bytes to be used for buffering across all threads

+ TYPE -- long
+ DEFAULT -- 10485760
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium

**client.id** -- An ID prefix string used for the client IDs of internal consumer, producer and restore-consumer, with pattern "-StreamThread--"

+ TYPE -- string
+ DEFAULT -- ""
+ IMPORTANCE -- medium

**default.deserialization.exception.handler** -- Exception handling class that implements the *org.apache.kafka.streams.errors.DeserializationExceptionHandler* interface

+ TYPE -- class
+ DEFAULT -- org.apache.kafka.streams.errors.LogAndFailExceptionHandler
+ IMPORTANCE -- medium

**default.key.serde** -- Default serializer / deserializer class for key that implements the *org.apache.kafka.common.serialization.Serde* interface

+ TYPE -- class
+ DEFAULT -- org.apache.kafka.common.serialization.Serdes$ByteArraySerde
+ IMPORTANCE -- medium

**default.timestamp.extractor** -- Default timestamp extractor class that implements the *org.apache.kafka.streams.processor.TimestampExtractor* interface

+ TYPE -- class
+ DEFAULT -- org.apache.kafka.streams.processor.FailOnInvalidTimestamp
+ IMPORTANCE -- medium

**default.value.serde** -- Default serializer/deserializer class for value that implements the *org.apache.kafka.common.serialization.Serde* interface

+ TYPE -- class
+ DEFAULT -- org.apache.kafka.common.serialization.Serdes$ByteArraySerde
+ IMPORTANCE -- medium

**num.standby.replicas** -- The number of standby replicas for each task

+ TYPE -- int
+ DEFAULT -- 0
+ IMPORTANCE -- medium

**num.stream.threads** -- The number of threads to execute stream processing

+ TYPE -- int
+ DEFAULT -- 1
+ IMPORTANCE -- medium

**processing.guarantee** -- The processing guarantee that should be used. Possible values are "at_least_once" (default) and "exactly_once". Note that "exactly-once" processing requires a cluster of at least three brokers by default what is the recommended setting for production; for development you can change this, by adjusting broker setting *transaction.state.log.replication.factor*

+ TYPE -- string
+ DEFAULT -- at_least_once
+ VALID VALUES -- [at_least_once, exactly_once]
+ IMPORTANCE -- medium

**security.protocol** -- Protocol used to communicate with brokers. Valid values are: "PLAINTEXT", "SSL", "SASL_PLAINTEXT", "SASL_SSL"

+ TYPE -- string
+ DEFAULT -- PLAINTEXT
+ IMPORTANCE -- medium

**application.server** -- A host:port pair pointing to an embedded user defined endpoint that can be used for discovering the locations of state stores within a single KafkaStreams application

+ TYPE -- string
+ DEFAULT -- ""
+ IMPORTANCE -- low

**buffered.records.per.partition** -- The maximum number of records to buffer per partition

+ TYPE -- int
+ DEFAULT -- 1000
+ IMPORTANCE -- low

**commit.interval.ms** -- The frequency with which to save the position of the processor. Note, if *processing.guarantee* is set to "exactly_once", the default value is "100", otherwise the default value is "30000"

+ TYPE -- long
+ DEFAULT -- 30000
+ IMPORTANCE -- low

**connections.max.idle.ms** -- Close idle connections after the number of milliseconds specified by this config

+ TYPE -- long
+ DEFAULT -- 540000
+ IMPORTANCE -- low

**key.serde** -- Serializer/deserializer class for key that implements the *org.apache.kafka.common.serialization.Serde* interface. This config is deprecated, use *default.key.serde* instead

+ TYPE -- class
+ DEFAULT -- null
+ IMPORTANCE -- low

**metadata.max.age.ms** -- The period of time in milliseconds after which we force a refresh of metadata even if we haven't seen any partition leadership changes to proactively discover any new brokers or partitions

+ TYPE -- long
+ DEFAULT -- 300000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**metric.reporters** -- A list of classes to use as metrics reporters. Implementing the *org.apache.kafka.common.metrics.MetricsReporter* interface allows plugging in classes that will be notified of new metric creation. The JmxReporter is always included to register JMX statistics

+ TYPE -- list
+ DEFAULT -- ""
+ IMPORTANCE -- low

**metrics.num.samples** -- The number of samples maintained to compute metrics

+ TYPE -- int
+ DEFAULT -- 2
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low

**metrics.recording.level** -- The highest recording level for metrics

+ TYPE -- string
+ DEFAULT -- INFO
+ VALID VALUES -- [INFO, DEBUG]
+ IMPORTANCE -- low

**metrics.sample.window.ms** -- The window of time a metrics sample is computed over

+ TYPE -- long
+ DEFAULT -- 30000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**partition.grouper** -- Partition grouper class that implements the *org.apache.kafka.streams.processor.PartitionGrouper* interface

+ TYPE -- class
+ DEFAULT -- org.apache.kafka.streams.processor.DefaultPartitionGrouper
+ IMPORTANCE -- low

**poll.ms** -- The amount of time in milliseconds to block waiting for input

+ TYPE -- long
+ DEFAULT -- 100
+ IMPORTANCE -- low

**receive.buffer.bytes** -- The size of the TCP receive buffer (*SO_RCVBUF*) to use when reading data. If the value is "-1", the OS default will be used

+ TYPE -- int
+ DEFAULT -- 32768
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**reconnect.backoff.max.ms** -- The maximum amount of time in milliseconds to wait when reconnecting to a broker that has repeatedly failed to connect. If provided, the backoff per host will increase exponentially for each consecutive connection failure, up to this maximum. After calculating the backoff increase, *20%* random jitter is added to avoid connection storms

+ TYPE -- long
+ DEFAULT -- 1000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**reconnect.backoff.ms** -- The base amount of time to wait before attempting to reconnect to a given host. This avoids repeatedly connecting to a host in a tight loop. This backoff applies to all connection attempts by the client to a broker

+ TYPE -- long
+ DEFAULT -- 50
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**request.timeout.ms** -- The configuration controls the maximum amount of time the client will wait for the response of a request. If the response is not received before the timeout elapses the client will resend the request if necessary or fail the request if retries are exhausted

+ TYPE -- int
+ DEFAULT -- 40000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**retry.backoff.ms** -- The amount of time to wait before attempting to retry a failed request to a given topic partition. This avoids repeatedly sending requests in a tight loop under some failure scenarios

+ TYPE -- long
+ DEFAULT -- 100
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**rocksdb.config.setter** -- A Rocks DB config setter class or class name that implements the *org.apache.kafka.streams.state.RocksDBConfigSetter* interface

+ TYPE -- class
+ DEFAULT -- null
+ IMPORTANCE -- low

**send.buffer.bytes** -- The size of the TCP send buffer (*SO_SNDBUF*) to use when sending data. If the value is "-1", the OS default will be used

+ TYPE -- int
+ DEFAULT -- 131072
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**state.cleanup.delay.ms** -- The amount of time in milliseconds to wait before deleting state when a partition has migrated. Only state directories that have not been modified for at least *state.cleanup.delay.ms* will be removed

+ TYPE -- long
+ DEFAULT -- 600000
+ IMPORTANCE -- low

**timestamp.extractor** -- Timestamp extractor class that implements the *org.apache.kafka.streams.processor.TimestampExtractor* interface. This config is deprecated, use *default.timestamp.extractor* instead

+ TYPE -- class
+ DEFAULT -- null
+ IMPORTANCE -- low

**value.serde** -- Serializer/deserializer class for value that implements the *org.apache.kafka.common.serialization.Serde* interface. This config is deprecated, use *default.value.serde* instead

+ TYPE -- class
+ DEFAULT -- null
+ IMPORTANCE -- low

**windowstore.changelog.additional.retention.ms** -- Added to a windows *maintainMs* to ensure data is not deleted from the log prematurely. Allows for clock drift. Default is *1 day*

+ TYPE -- long
+ DEFAULT -- 86400000
+ IMPORTANCE -- low

**zookeeper.connect** -- Zookeeper connect string for ADS topics management. This config is deprecated and will be ignored as Streams API does not use Zookeeper anymore

+ TYPE -- string
+ DEFAULT -- ""
+ IMPORTANCE -- low


