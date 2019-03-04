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

**partition.grouper** -- Класс Partition grouper, реализующий интерфейс *org.apache.kafka.streams.processor.PartitionGrouper*

+ TYPE -- class
+ DEFAULT -- org.apache.kafka.streams.processor.DefaultPartitionGrouper
+ IMPORTANCE -- low

**poll.ms** -- Время блокировки ожидания ввода. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 100
+ IMPORTANCE -- low

**receive.buffer.bytes** -- Размер буфера приема TCP (SO_RCVBUF) при чтении данных. Если значение равно "-1", используется ОС по умолчанию

+ TYPE -- int
+ DEFAULT -- 32768
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**reconnect.backoff.max.ms** -- Максимальный период времени ожидания повторного подключения к брокеру при неоднократных сбоях соединения. Отсрочка на хост увеличивается экспоненциально для каждого последующего сбоя соединения, вплоть до установленного максимума. После расчета увеличения отсрочки к значению добавляется *20%* случайного джиттера во избежание помех связи. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 1000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**reconnect.backoff.ms** -- Базовый период времени ожидания повторного подключения к хосту. Позволяет избегать многократного подключения к узлу в узком цикле. Данная отсрочка применяется ко всем попыткам подключения клиента к брокеру. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 50
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**request.timeout.ms** -- Максимальное время ожидания клиентом ответа на запрос. Если ответ не получен до истечения установленного значения, клиент повторно отправляет запрос при необходимости. Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- 40000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**retries** -- Установка значения больше нуля приводит к тому, что клиент переотправляет любую запись, передача которой завершается с временной ошибкой

+ TYPE -- int
+ DEFAULT -- 0
+ VALID VALUES -- [0,...,2147483647]
+ IMPORTANCE -- low

**retry.backoff.ms** -- Время ожидания перед повторной попыткой отправки неудавшегося запроса в партицию топика. Позволяет избежать неоднократной отправки запросов в сжатом цикле. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 100
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**rocksdb.config.setter** -- Класс или имя класса установщика конфигурации базы данных Rocks, реализующий интерфейс *org.apache.kafka.streams.state.RocksDBConfigSetter*

+ TYPE -- class
+ DEFAULT -- null
+ IMPORTANCE -- low

**send.buffer.bytes** -- Размер буфера отправки TCP (SO_SNDBUF) при отправке данных. Если значение равно "-1", используется ОС по умолчанию

+ TYPE -- int
+ DEFAULT -- 131072
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**state.cleanup.delay.ms** -- Время ожидания перед удалением state каталогов после перемещения партиции. Удаляются только state каталоги, которые не были изменены. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 600000
+ IMPORTANCE -- low

**timestamp.extractor** -- Класс выделения временных меток, реализующий интерфейс *org.apache.kafka.streams.processor.TimestampExtractor*. Данная конфигурация устарела, вместо нее используется *default.timestamp.extractor*

+ TYPE -- class
+ DEFAULT -- null
+ IMPORTANCE -- low

**value.serde** -- Класс сериализатора/десериализатора для значения, реализующего интерфейс *org.apache.kafka.common.serialization.Serde*. Данная конфигурация устарела, вместо нее используется *ddefault.value.serde*

+ TYPE -- class
+ DEFAULT -- null
+ IMPORTANCE -- low

**windowstore.changelog.additional.retention.ms** -- Добавление *maintainMs* с целью исключения риска преждевременного удаления данных из журнала. Позволяет осуществлять отставание часов. По умолчанию устанавливается 1 день. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 86400000
+ IMPORTANCE -- low

**zookeeper.connect** -- Соединение строки для управления топиками ADS через Zookeeper. Конфигурация устарела и игнорируется, поскольку Streams API больше не использует Zookeeper

+ TYPE -- string
+ DEFAULT -- ""
+ IMPORTANCE -- low


