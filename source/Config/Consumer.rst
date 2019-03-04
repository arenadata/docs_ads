Consumer Configs
==========================

Below is the configuration for the new consumer.

**key.deserializer** -- Deserializer class for key that implements the *org.apache.kafka.common.serialization.Deserializer* interface

+ TYPE -- class
+ IMPORTANCE -- high

**value.deserializer** -- Deserializer class for value that implements the *org.apache.kafka.common.serialization.Deserializer* interface

+ TYPE -- class
+ IMPORTANCE -- high

**fetch.min.bytes** -- The minimum amount of data the server should return for a fetch request. If insufficient data is available the request will wait for that much data to accumulate before answering the request. The default setting of "1" byte means that fetch requests are answered as soon as a single byte of data is available or the fetch request times out waiting for data to arrive. Setting this to something greater than "1" will cause the server to wait for larger amounts of data to accumulate which can improve server throughput a bit at the cost of some additional latency

+ TYPE -- int
+ DEFAULT -- 1
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- high

**group.id** -- A unique string that identifies the consumer group this consumer belongs to. This property is required if the consumer uses either the group management functionality by using *subscribe(topic)* or the ADS-based offset management strategy

+ TYPE -- string
+ DEFAULT -- ""
+ IMPORTANCE -- high

**heartbeat.interval.ms** -- The expected time between heartbeats to the consumer coordinator when using ADS's group management facilities. Heartbeats are used to ensure that the consumer's session stays active and to facilitate rebalancing when new consumers join or leave the group. The value must be set lower than *session.timeout.ms*, but typically should be set no higher than 1/3 of that value. It can be adjusted even lower to control the expected time for normal rebalances

+ TYPE -- int
+ DEFAULT -- 3000
+ IMPORTANCE -- high

**max.partition.fetch.bytes** -- The maximum amount of data per-partition the server will return. Records are fetched in batches by the consumer. If the first record batch in the first non-empty partition of the fetch is larger than this limit, the batch will still be returned to ensure that the consumer can make progress. The maximum record batch size accepted by the broker is defined via *message.max.bytes* (broker config) or *max.message.bytes* (topic config). See *fetch.max.bytes* for limiting the consumer request size

+ TYPE -- int
+ DEFAULT -- 1048576
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- high

**session.timeout.ms** -- The timeout used to detect consumer failures when using ADS's group management facility. The consumer sends periodic heartbeats to indicate its liveness to the broker. If no heartbeats are received by the broker before the expiration of this session timeout, then the broker will remove this consumer from the group and initiate a rebalance. Note that the value must be in the allowable range as configured in the broker configuration by *group.min.session.timeout.ms* and *group.max.session.timeout.ms*

+ TYPE -- int
+ DEFAULT -- 10000
+ IMPORTANCE -- high

**ssl.key.password** -- The password of the private key in the key store file. This is optional for client

+ TYPE -- password
+ DEFAULT -- null
+ IMPORTANCE -- high

**ssl.keystore.location** -- The location of the key store file. This is optional for client and can be used for two-way authentication for client

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- high

**ssl.keystore.password** -- The store password for the key store file. This is optional for client and only needed if *ssl.keystore.location* is configured

+ TYPE -- password
+ DEFAULT -- null
+ IMPORTANCE -- high

**ssl.truststore.location** -- The location of the trust store file

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- high

**ssl.truststore.password** -- The password for the trust store file. If a password is not set access to the truststore is still available, but integrity checking is disabled

+ TYPE -- password
+ DEFAULT -- null
+ IMPORTANCE -- null

**auto.offset.reset** -- What to do when there is no initial offset in ADS or if the current offset does not exist any more on the server (e.g. because that data has been deleted):

  + *earliest*: automatically reset the offset to the earliest offset;
  + *latest*: automatically reset the offset to the latest offset;
  + *none*: throw exception to the consumer if no previous offset is found for the consumer's group;
  + *anything else*: throw exception to the consumer.

+ TYPE -- string
+ DEFAULT -- latest
+ VALID VALUES -- [latest, earliest, none]
+ IMPORTANCE -- medium

**connections.max.idle.ms** -- Close idle connections after the number of milliseconds specified by this config

+ TYPE -- long
+ DEFAULT -- 540000
+ IMPORTANCE -- medium

**enable.auto.commit** -- If "true" the consumer's offset will be periodically committed in the background

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- medium

**exclude.internal.topics** -- Whether records from internal topics (such as offsets) should be exposed to the consumer. If set to "true" the only way to receive records from an internal topic is subscribing to it

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- medium

**fetch.max.bytes** -- The maximum amount of data the server should return for a fetch request. Records are fetched in batches by the consumer, and if the first record batch in the first non-empty partition of the fetch is larger than this value, the record batch will still be returned to ensure that the consumer can make progress. As such, this is not a absolute maximum. The maximum record batch size accepted by the broker is defined via *message.max.bytes* (broker config) or *max.message.bytes* (topic config). Note that the consumer performs multiple fetches in parallel 

+ TYPE -- int
+ DEFAULT -- 52428800
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium

**isolation.level** -- Controls how to read messages written transactionally. If set to "read_committed", *consumer.poll()* will only return transactional messages which have been committed. If set to "read_uncommitted" (the default), *consumer.poll()* will return all messages, even transactional messages which have been aborted. Non-transactional messages will be returned unconditionally in either mode. Messages will always be returned in offset order. Hence, in "read_committed" mode, *consumer.poll()* will only return messages up to the last stable offset (LSO), which is the one less than the offset of the first open transaction. In particular any messages appearing after messages belonging to ongoing transactions will be withheld until the relevant transaction has been completed. As a result, "read_committed" consumers will not be able to read up to the high watermark when there are in flight transactions. Further, when in "read_committed" the *seekToEnd* method will return the LSO

+ TYPE -- string
+ DEFAULT -- read_uncommitted
+ VALID VALUES -- [read_committed, read_uncommitted]
+ IMPORTANCE -- medium

**max.poll.interval.ms** -- The maximum delay between invocations of *poll()* when using consumer group management. This places an upper bound on the amount of time that the consumer can be idle before fetching more records. If *poll()* is not called before expiration of this timeout, then the consumer is considered failed and the group will rebalance in order to reassign the partitions to another member

+ TYPE -- int
+ DEFAULT -- 300000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- medium

**max.poll.records** -- The maximum number of records returned in a single call to *poll()*

+ TYPE -- int
+ DEFAULT -- 500
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- medium

**partition.assignment.strategy** -- The class name of the partition assignment strategy that the client will use to distribute partition ownership amongst consumer instances when group management is used

+ TYPE -- list
+ DEFAULT -- class org.apache.kafka.clients.consumer.RangeAssignor
+ VALID VALUES -- org.apache.kafka.common.config.ConfigDef$NonNullValidator@6d4b1c02
+ IMPORTANCE -- medium

**receive.buffer.bytes** -- The size of the TCP receive buffer (*SO_RCVBUF*) to use when reading data. If the value is "-1", the OS default will be used

+ TYPE -- int
+ DEFAULT -- 65536
+ VALID VALUES -- [-1,...]
+ IMPORTANCE -- medium

**request.timeout.ms** -- The configuration controls the maximum amount of time the client will wait for the response of a request. If the response is not received before the timeout elapses the client will resend the request if necessary or fail the request if retries are exhausted

+ TYPE -- int
+ DEFAULT -- 305000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium

**sasl.jaas.config** -- JAAS login context parameters for SASL connections in the format used by JAAS configuration files. JAAS configuration file format is described  `here <http://docs.oracle.com/javase/8/docs/technotes/guides/security/jgss/tutorials/LoginConfigFile.html>`_. The format for the value is: "(=)*;"

+ TYPE -- password
+ DEFAULT -- null
+ IMPORTANCE -- medium

**sasl.kerberos.service.name** -- The Kerberos principal name that ADS runs as. This can be defined either in ADS's JAAS config or in ADS's config

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- medium

**sasl.mechanism** -- SASL mechanism used for client connections. This may be any mechanism for which a security provider is available. GSSAPI is the default mechanism

+ TYPE -- string
+ DEFAULT -- GSSAPI
+ IMPORTANCE -- medium

**security.protocol** -- Protocol used to communicate with brokers. Valid values are: "PLAINTEXT", "SSL", "SASL_PLAINTEXT", "SASL_SSL"

+ TYPE -- string
+ DEFAULT -- PLAINTEXT
+ IMPORTANCE -- medium

**send.buffer.bytes** -- The size of the TCP send buffer (*SO_SNDBUF*) to use when sending data. If the value is "-1", the OS default will be used

+ TYPE -- int
+ DEFAULT -- 131072
+ VALID VALUES -- [-1,...]
+ IMPORTANCE -- medium

**ssl.enabled.protocols** -- The list of protocols enabled for SSL connections

+ TYPE -- list
+ DEFAULT -- TLSv1.2,TLSv1.1,TLSv1
+ IMPORTANCE -- medium

**ssl.keystore.type** -- The file format of the key store file. This is optional for client

+ TYPE -- string
+ DEFAULT -- JKS
+ IMPORTANCE -- medium

**ssl.protocol** -- The SSL protocol used to generate the SSLContext. Default setting is "TLS", which is fine for most cases. Allowed values in recent JVMs are "TLS", "TLSv1.1" and "TLSv1.2". "SSL", "SSLv2" and "SSLv3" may be supported in older JVMs, but their usage is discouraged due to known security vulnerabilities

+ TYPE -- string
+ DEFAULT -- TLS
+ IMPORTANCE -- medium

**ssl.provider** -- The name of the security provider used for SSL connections. Default value is the default security provider of the JVM

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- medium

**ssl.truststore.type** -- The file format of the trust store file

+ TYPE -- string
+ DEFAULT -- JKS
+ IMPORTANCE -- medium

**auto.commit.interval.ms** -- The frequency in milliseconds that the consumer offsets are auto-committed to ADS if *enable.auto.commit* is set to "true"

+ TYPE -- int
+ DEFAULT -- 5000
+ VALID VALUES -- [0,...]	
+ IMPORTANCE -- low

**check.crcs** -- Automatically check the CRC32 of the records consumed. This ensures no on-the-wire or on-disk corruption to the messages occurred. This check adds some overhead, so it may be disabled in cases seeking extreme performance

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- low

**client.id** -- An id string to pass to the server when making requests. The purpose of this is to be able to track the source of requests beyond just ip/port by allowing a logical application name to be included in server-side request logging

+ TYPE -- string	
+ DEFAULT -- ""
+ IMPORTANCE -- low

**fetch.max.wait.ms** -- The maximum amount of time the server will block before answering the fetch request if there isn't sufficient data to immediately satisfy the requirement given by *fetch.min.bytes*

+ TYPE -- int
+ DEFAULT -- 500
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**interceptor.classes** -- A list of classes to use as interceptors. Implementing the *org.apache.kafka.clients.consumer.ConsumerInterceptor* interface allows you to intercept (and possibly mutate) records received by the consumer. By default, there are no interceptors

+ TYPE -- list
+ DEFAULT -- ""
+ VALID VALUES -- org.apache.kafka.common.config.ConfigDef$NonNullValidator@6093dd95
+ IMPORTANCE -- low

**metadata.max.age.ms** -- The period of time in milliseconds after which we force a refresh of metadata even if we haven't seen any partition leadership changes to proactively discover any new brokers or partitions

+ TYPE -- long
+ DEFAULT -- 300000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**metric.reporters** -- A list of classes to use as metrics reporters. Implementing the *org.apache.kafka.common.metrics.MetricsReporter* interface allows plugging in classes that will be notified of new metric creation. The JmxReporter is always included to register JMX statistics

+ TYPE -- list
+ DEFAULT -- ""
+ VALID VALUES -- org.apache.kafka.common.config.ConfigDef$NonNullValidator@5622fdf
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

**retry.backoff.ms** -- The amount of time to wait before attempting to retry a failed request to a given topic partition. This avoids repeatedly sending requests in a tight loop under some failure scenarios

+ TYPE -- long
+ DEFAULT -- 100
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**sasl.kerberos.kinit.cmd** -- Kerberos kinit command path

+ TYPE -- string
+ DEFAULT -- /usr/bin/kinit
+ IMPORTANCE -- low

**sasl.kerberos.min.time.before.relogin** -- Login thread sleep time between refresh attempts

+ TYPE -- long
+ DEFAULT -- 60000
+ IMPORTANCE -- low

**sasl.kerberos.ticket.renew.jitter** -- Percentage of random jitter added to the renewal time

+ TYPE -- double
+ DEFAULT -- 0.05
+ IMPORTANCE -- low

**sasl.kerberos.ticket.renew.window.factor** -- Login thread will sleep until the specified window factor of time from last refresh to ticket's expiry has been reached, at which time it will try to renew the ticket

+ TYPE -- double
+ DEFAULT -- 0.8
+ IMPORTANCE -- low

**ssl.cipher.suites** -- A list of cipher suites. This is a named combination of authentication, encryption, MAC and key exchange algorithm used to negotiate the security settings for a network connection using TLS or SSL network protocol. By default all the available cipher suites are supported

+ TYPE -- list
+ DEFAULT -- null
+ IMPORTANCE -- low

**ssl.endpoint.identification.algorithm** -- The endpoint identification algorithm to validate server hostname using server certificate

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- low

**ssl.keymanager.algorithm** -- The algorithm used by key manager factory for SSL connections. Default value is the key manager factory algorithm configured for the Java Virtual Machine

+ TYPE -- string
+ DEFAULT -- SunX509
+ IMPORTANCE -- low

**ssl.secure.random.implementation** -- The SecureRandom PRNG implementation to use for SSL cryptography operations

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- low

**ssl.trustmanager.algorithm** -- The algorithm used by trust manager factory for SSL connections. Default value is the trust manager factory algorithm configured for the Java Virtual Machine

+ TYPE -- string
+ DEFAULT -- PKIX
+ IMPORTANCE -- low

