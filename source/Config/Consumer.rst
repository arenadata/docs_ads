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

**send.buffer.bytes** -- Буфер SO_SNDBUF сокета сервера сокетов. При значении параметра "-1" используется ОС по умолчанию

+ TYPE -- int
+ DEFAULT -- 131072
+ VALID VALUES -- [-1,...]
+ IMPORTANCE -- medium

**ssl.enabled.protocols** -- Список протоколов, включенных для соединений SSL

+ TYPE -- list
+ DEFAULT -- TLSv1.2,TLSv1.1,TLSv1
+ IMPORTANCE -- medium

**ssl.keystore.type** -- Формат файла хранилища ключей. Необязательный параметр для клиента

+ TYPE -- string
+ DEFAULT -- JKS
+ IMPORTANCE -- medium

**ssl.protocol** -- Протокол SSL для генерации SSLContext. Значение по умолчанию – "TLS", что подходит для большинства случаев. Допустимыми значениями в последних JVM являются "TLS", "TLSv1.1" и "TLSv1.2". Протоколы "SSL", "SSLv2" и "SSLv3" могут поддерживаться в более старых JVM, но их использование не рекомендуется из-за известных уязвимостей безопасности

+ TYPE -- string
+ DEFAULT -- TLS
+ IMPORTANCE -- medium

**ssl.provider** -- Имя поставщика безопасности для соединений SSL. Значением по умолчанию является поставщик безопасности по умолчанию для JVM

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- medium

**ssl.truststore.type** -- Формат файла хранилища trust store

+ TYPE -- string
+ DEFAULT -- JKS
+ IMPORTANCE -- medium

**auto.commit.interval.ms** -- Частота автофиксации потребительских смещений при включенном параметре *enable.auto.commit* (значение "true"). Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- 5000
+ VALID VALUES -- [0,...]	
+ IMPORTANCE -- low

**check.crcs** -- Автоматическая проверка CRC32 считываемых записей. Проверка добавляет некоторые накладные расходы, поэтому она может быть отключена в случаях, требующих высокой производительности

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- low

**client.id** -- Строка id для передачи на сервер при выполнении запросов. Целью является возможность отслеживания источника запросов за пределами ip/port, позволяя включать логическое имя приложения в журнал запросов на стороне сервера

+ TYPE -- string	
+ DEFAULT -- ""
+ IMPORTANCE -- low

**fetch.max.wait.ms** -- Максимальный период времени, в течение которого сервер блокируется, прежде чем ответить на запрос выборки (в случае недостаточного объема данных для незамедлительного ответа, заданного функцией *fetch.min.bytes*). Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- 500
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**interceptor.classes** -- Список классов для использования в качестве интерсепторов. Реализация интерфейса *org.apache.kafka.clients.consumer.ConsumerInterceptor* позволяет перехватывать (и, возможно, видоизменять) полученные потребителем записи. По умолчанию интерсепторы не установлены

+ TYPE -- list
+ DEFAULT -- ""
+ VALID VALUES -- org.apache.kafka.common.config.ConfigDef$NonNullValidator@6093dd95
+ IMPORTANCE -- low

**metadata.max.age.ms** -- Период времени, после которого принудительно обновляются метаданные даже при отсутствии видимых изменений в лидере партиции с целью предварительного обнаружения новых брокеров или партиций. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 300000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**metric.reporters** -- Список классов для использования в качестве репортеров метрик. Реализация интерфейса *org.apache.kafka.common.metrics.MetricsReporter* позволяет подключать классы, которые будут уведомлены о создании новой метрики. JmxReporter всегда включен в реестр статистических данных JMX

+ TYPE -- list
+ DEFAULT -- ""
+ VALID VALUES -- org.apache.kafka.common.config.ConfigDef$NonNullValidator@5622fdf
+ IMPORTANCE -- low

**metrics.num.samples** -- Количество выборок, поддерживаемых для вычисления метрик

+ TYPE -- int
+ DEFAULT -- 2
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low

**metrics.recording.level** -- Самый высокий уровень записи для метрик

+ TYPE -- string
+ DEFAULT -- INFO
+ VALID VALUES -- [INFO, DEBUG]
+ IMPORTANCE -- low

**metrics.sample.window.ms** -- Время ожидания вычисления метрик выборки. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 30000
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

**retry.backoff.ms** -- Время ожидания перед повторной попыткой отправки неудавшегося запроса в партицию топика. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 100
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**sasl.kerberos.kinit.cmd** -- Путь команд Kerberos kinit

+ TYPE -- string
+ DEFAULT -- /usr/bin/kinit
+ IMPORTANCE -- low

**sasl.kerberos.min.time.before.relogin** -- Время ожидания авторизации потока между попытками обновления

+ TYPE -- long
+ DEFAULT -- 60000
+ IMPORTANCE -- low

**sasl.kerberos.ticket.renew.jitter** -- Процент случайного джиттера по отношению к времени возобновления

+ TYPE -- double
+ DEFAULT -- 0.05
+ IMPORTANCE -- low

**sasl.kerberos.ticket.renew.window.factor** -- Время ожидания авторизации потока до тех пор, пока не будет достигнут указанный коэффициент времени от последнего обновления до истечения срока действия тикета, и попытка возобновления тикета за этот период времени

+ TYPE -- double
+ DEFAULT -- 0.8
+ IMPORTANCE -- low

**ssl.cipher.suites** -- Список наборов шифров. Именованная комбинация аутентификации, шифрования, MAC и ключей обмена алгоритма для согласования параметров безопасности для сетевого подключения с использованием протокола TLS или SSL. По умолчанию поддерживаются все доступные варианты шифрования

+ TYPE -- list
+ DEFAULT -- null
+ IMPORTANCE -- low

**ssl.endpoint.identification.algorithm** -- Алгоритм идентификации конечных точек для валидации имени хоста сервера с использованием сертификата сервера

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- low

**ssl.keymanager.algorithm** -- Алгоритм службы управления ключами для SSL-соединений. Значением по умолчанию является алгоритм, настроенный для Java Virtual Machine

+ TYPE -- string
+ DEFAULT -- SunX509
+ IMPORTANCE -- low

**ssl.secure.random.implementation** -- Реализация SecureRandom PRNG, используемая для операций шифрования SSL

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- low

**ssl.trustmanager.algorithm** -- Алгоритм доверенной службы управления ключами для SSL-соединений. Значением по умолчанию является алгоритм, настроенный для Java Virtual Machine

+ TYPE -- string
+ DEFAULT -- PKIX
+ IMPORTANCE -- low

