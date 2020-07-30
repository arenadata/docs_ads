Конфигурирование Streams
=========================

Далее приведены конфигурации для клиентской библиотеки ADS Streams.

**application.id** -- Идентификатор приложения потоковой обработки. Должен быть уникальным в пределах платформы ADS. Используется как: 1) префикс идентификатора клиента по умолчанию, 2) идентификатор группы для управления членством, 3) префикс топика изменений в журнале

+ TYPE -- string
+ IMPORTANCE -- high

**bootstrap.servers** -- Список пар хост/порт, используемых для установления первоначального подключения к платформе ADS. В дальнейшем клиент будет использовать все сервера, независимо от того, какие указаны в данном параметре – этот список влияет только на начальные хосты, используемые для обнаружения полного набора серверов. Параметр должен быть задан в формате “host1:port1, host2:port2,...” (через запятую и без пробелов). Поскольку данные сервера используются только для первоначального подключения с целью обнаружения полного набора в кластере (который может динамически меняться), списку необязательно содержать полный набор серверов (можно указать более одного, на случай отказа первого)

+ TYPE -- list
+ IMPORTANCE -- high

**replication.factor** -- Коэффициент репликации для топиков журнала изменений и репартицирования топиков, созданных приложением потоковой обработки

+ TYPE -- int
+ DEFAULT -- 1
+ IMPORTANCE -- high

**state.dir** -- Местоположение каталога хранилища state store

+ TYPE -- string
+ DEFAULT -- /tmp/kafka-streams
+ IMPORTANCE -- high

**cache.max.bytes.buffering** -- Максимальный объем памяти в байтах, используемый для буферизации всех потоков

+ TYPE -- long
+ DEFAULT -- 10485760
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium

**client.id** -- Строка префикса ID, используемая для идентификаторов клиента внутреннего потребителя и поставщика (восстановления потребителя); шаблон "-StreamThread--"

+ TYPE -- string
+ DEFAULT -- ""
+ IMPORTANCE -- medium

**default.deserialization.exception.handler** -- Класс обработки исключений, реализующий интерфейс *org.apache.kafka.streams.errors.DeserializationExceptionHandler*

+ TYPE -- class
+ DEFAULT -- org.apache.kafka.streams.errors.LogAndFailExceptionHandler
+ IMPORTANCE -- medium

**default.key.serde** -- Класс сериализатора/десериализатора по умолчанию для ключа, реализующего интерфейс *org.apache.kafka.common.serialization.Serde*

+ TYPE -- class
+ DEFAULT -- org.apache.kafka.common.serialization.Serdes$ByteArraySerde
+ IMPORTANCE -- medium

**default.production.exception.handler** -- Класс обработки исключений, реализующий интерфейс *org.apache.kafka.streams.errors.ProductionExceptionHandler*

+ TYPE -- class
+ DEFAULT -- org.apache.kafka.streams.errors.DefaultProductionExceptionHandler
+ IMPORTANCE -- medium

**default.timestamp.extractor** -- Класс выделения временных меток по умолчанию, реализующий интерфейс *org.apache.kafka.streams.processor.TimestampExtractor*

+ TYPE -- class
+ DEFAULT -- org.apache.kafka.streams.processor.FailOnInvalidTimestamp
+ IMPORTANCE -- medium

**default.value.serde** -- Класс сериализатора/десериализатора по умолчанию для значения, реализующего интерфейс *org.apache.kafka.common.serialization.Serde*

+ TYPE -- class
+ DEFAULT -- org.apache.kafka.common.serialization.Serdes$ByteArraySerde
+ IMPORTANCE -- medium

**num.standby.replicas** -- Число резервных реплик для каждой задачи

+ TYPE -- int
+ DEFAULT -- 0
+ IMPORTANCE -- medium

**num.stream.threads** -- Количество потоков для выполнения потоковой обработки

+ TYPE -- int
+ DEFAULT -- 1
+ IMPORTANCE -- medium

**processing.guarantee** -- Гарантия на обработку. Возможные значения: "at_least_once" (по умолчанию) и "exact_once". Для обработки "exact_once" требуется по меньшей мере три брокера по умолчанию, что является рекомендуемой настройкой для продуктивной среды; для разработки можно изменить параметр, при этом переустановив настройку брокера *transaction.state.log.replication.factor*

+ TYPE -- string
+ DEFAULT -- at_least_once
+ VALID VALUES -- [at_least_once, exactly_once]
+ IMPORTANCE -- medium

**security.protocol** -- Протокол безопасности для связи между брокерами. Допустимые значения: "PLAINTEXT", "SSL", "SASL_PLAINTEXT", "SASL_SSL"

+ TYPE -- string
+ DEFAULT -- PLAINTEXT
+ IMPORTANCE -- medium

**application.server** -- Пара "host:port", указывающая на встроенную конечную точку пользователя, которая может использоваться для обнаружения местоположений хранилищ state stores в рамках одного приложения

+ TYPE -- string
+ DEFAULT -- ""
+ IMPORTANCE -- low

**buffered.records.per.partition** -- Максимальное количество записей в буфере для каждой партиции

+ TYPE -- int
+ DEFAULT -- 1000
+ IMPORTANCE -- low

**commit.interval.ms** -- Частота сохранения положения процессора. Если параметр *processing.guarantee* установлен на "exactly_once", значение по умолчанию равно "100", иначе (при "at_least_once") значение по умолчанию равно "30000". Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 30000
+ IMPORTANCE -- low

**connections.max.idle.ms** -- Закрытие бездействующих соединений по истечению заданного периода. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 540000
+ IMPORTANCE -- low

**key.serde** -- Сериализатор/десериализатор для ключа, реализующего интерфейс *org.apache.kafka.common.serialization.Serde*. Данная конфигурация устарела, вместо нее используется *default.key.serde*

+ TYPE -- class
+ DEFAULT -- null
+ IMPORTANCE -- low

**metadata.max.age.ms** -- Период времени, после которого принудительно обновляются метаданные даже при отсутствии видимых изменений в лидере партиции с целью предварительного обнаружения новых брокеров или партиций. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 300000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- low

**metric.reporters** -- Список классов для использования в качестве репортеров метрик. Реализация интерфейса *org.apache.kafka.common.metrics.MetricsReporter* позволяет подключать классы, которые будут уведомлены о создании новой метрики. JmxReporter всегда включен в реестр статистических данных JMX

+ TYPE -- list
+ DEFAULT -- ""
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


