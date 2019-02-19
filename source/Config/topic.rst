Topic-Level Configs
======================


Configurations pertinent to topics have both a server default as well an optional per-topic override. If no per-topic configuration is given the server default is used. The override can be set at topic creation time by giving one or more *--config* options. 

The following are the topic-level configurations. The server's default configuration for this property is given under the Server Default Property heading. A given server default config value only applies to a topic if it does not have an explicit topic config override.

**cleanup.policy** -- A string that is either "delete" or "compact". This string designates the retention policy to use on old log segments. The default policy ("delete") will discard old segments when their retention time or size limit has been reached. The "compact" setting will enable log compaction on the topic

+ TYPE -- list
+ DEFAULT -- delete
+ VALID VALUES -- [compact, delete]
+ SERVER DEFAULT PROPERTY --log.cleanup.policy
+ IMPORTANCE -- medium

**compression.type** -- Specify the final compression type for a given topic. This configuration accepts the standard compression codecs ("gzip", "snappy", "lz4"). It additionally accepts "uncompressed" which is equivalent to no compression; and "producer" which means retain the original compression codec set by the producer

+ TYPE -- string
+ DEFAULT -- producer
+ VALID VALUES -- [uncompressed, snappy, lz4, gzip, producer]
+ SERVER DEFAULT PROPERTY -- compression.type
+ IMPORTANCE -- medium

**delete.retention.ms** -- The amount of time to retain delete tombstone markers for log compacted topics. This setting also gives a bound on the time in which a consumer must complete a read if they begin from offset "0" to ensure that they get a valid snapshot of the final stage (otherwise delete tombstones may be collected before they complete their scan)

+ TYPE -- long
+ DEFAULT -- 86400000
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.cleaner.delete.retention.ms
+ IMPORTANCE -- medium

**file.delete.delay.ms** -- The time to wait before deleting a file from the filesystem

+ TYPE -- long
+ DEFAULT -- 60000
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.segment.delete.delay.ms
+ IMPORTANCE -- medium

**flush.messages** -- This setting allows specifying an interval at which we will force an fsync of data written to the log. For example if this was set to "1" we would fsync after every message; if it were "5" we would fsync after every five messages. In general we recommend you not set this and use replication for durability and allow the operating system's background flush capabilities as it is more efficient. This setting can be overridden on a per-topic basis 

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.flush.interval.messages
+ IMPORTANCE -- medium

**flush.ms** -- This setting allows specifying a time interval at which we will force an fsync of data written to the log. For example if this was set to "1000" we would fsync after 1000 ms had passed. In general we recommend you not set this and use replication for durability and allow the operating system's background flush capabilities as it is more efficient

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.flush.interval.ms
+ IMPORTANCE -- medium

**follower.replication.throttled.replicas** -- A list of replicas for which log replication should be throttled on the follower side. The list should describe a set of replicas in the form "[PartitionId]:[BrokerId],[PartitionId]:[BrokerId]:..." or alternatively the wildcard "*" can be used to throttle all replicas for this topic

+ TYPE -- list
+ DEFAULT -- ""
+ VALID VALUES -- [partitionId],[brokerId]:[partitionId],[brokerId]:...
+ SERVER DEFAULT PROPERTY -- follower.replication.throttled.replicas
+ IMPORTANCE -- medium

**index.interval.bytes** -- Частота добавления индексной записи в индекс смещения. Значение по умолчанию гарантирует индексацию сообщения примерно каждые 4096 байт. Большее индексирование позволяет потребителям приближаться к более точному положению в журнале, но увеличивает сам индекс. Рекомендуется значение не менять

+ TYPE -- int
+ DEFAULT -- 4096
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.index.interval.bytes
+ IMPORTANCE -- medium

**leader.replication.throttled.replicas** -- Список реплик, для которых репликация журнала должна дросселироваться на стороне лидера. Список должен описывать набор реплик в формате "[PartitionId]:[BrokerId],[PartitionId]:[BrokerId]:..." или можно использовать специальный символ "*" для дросселирования всех реплик в данном топике

+ TYPE -- list
+ DEFAULT -- ""
+ VALID VALUES -- [partitionId],[brokerId]:[partitionId],[brokerId]:...
+ SERVER DEFAULT PROPERTY -- leader.replication.throttled.replicas
+ IMPORTANCE -- medium

**max.message.bytes** -- Наибольший размер пакета данных, разрешенный ADS. При увеличении параметра следует также увеличить размер выборки для потребителей с целью обеспечения возможности получения пакета данных установленного размера

+ TYPE -- int
+ DEFAULT -- 1000012
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- message.max.bytes
+ IMPORTANCE -- medium

**message.format.version** -- Версия формата сообщений, которую брокер использует для добавления данных в журналы. Значение должно быть действительным ApiVersion. Некоторые примеры: “0.8.2”, “0.9.0.0”, “0.10.0”. Необходимо проверить ApiVersion для получения более подробной информации. Установив версию формата сообщений, пользователь подтверждает, что все существующие данные на диске меньше или равны указанной версии. Неправильное задание параметра приводит к тому, что потребители с более старыми версиями получают данные в нечитаемом формате

+ TYPE -- string
+ DEFAULT -- 1.1-IV0
+ SERVER DEFAULT PROPERTY -- log.message.format.version
+ IMPORTANCE -- medium

**message.timestamp.difference.max.ms** -- Максимальное допустимое различие между отметкой времени, когда брокер получает сообщение, и отметкой времени, указанной в сообщении. При *message.timestamp.type=CreateTime* сообщение отклоняется, если разница в отметке времени превышает указанный порог. Конфигурация игнорируется, если *message.timestamp.type=LogAppendTime*. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.message.timestamp.difference.max.ms
+ IMPORTANCE -- medium

**message.timestamp.type** -- Определить, является ли отметка времени в сообщении временем создания сообщения или временем добавления журнала. Параметр может принимать значение "CreateTime" либо "LogAppendTime"

+ TYPE -- string
+ DEFAULT -- CreateTime
+ SERVER DEFAULT PROPERTY -- log.message.timestamp.type
+ IMPORTANCE -- medium

**min.cleanable.dirty.ratio** -- Частота очистки журнала (при условии включенного сжатия). По умолчанию избегается очистка, где сжато более 50% журнала. Это ограничивает максимальное пространство, выделенное в журнале на дубликаты (не более 50% журнала могут занимать дубликаты). Более высокое отношение означает меньшее количество дубликатов и более эффективную очистку, но при этом большее количество потерянного пространства в журнале

+ TYPE -- double
+ DEFAULT -- 0.5
+ VALID VALUES -- [0,...,1]
+ SERVER DEFAULT PROPERTY -- log.cleaner.min.cleanable.ratio
+ IMPORTANCE -- medium

**min.compaction.lag.ms** -- Минимальное время, в течение которого сообщение остается несжатым в журнале. Применяется только для журналов с функцией сжатия. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 0
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.cleaner.min.compaction.lag.ms
+ IMPORTANCE -- medium

**min.insync.replicas** -- При установленном поставщиком подтверждении acks на "all" или "-1", *min.insync.replicas* задается на минимальное количество реплик для подтверждения записи. Если этот минимум не может быть удовлетворен, то поставщик задает исключение (*NotEnoughReplicas* или *NotEnoughReplicasAfterAppend*). Совместное использование *min.insync.replicas* и acks обеспечивает более высокую гарантию к устойчивости. Типичным сценарием является создание топика с коэффициентом репликации *3*, параметром *min.insync.replicas* равным *2* и acks установленным на "all". Это гарантирует, что поставщик задает исключение, если большинство реплик не принимает запись

+ TYPE -- int
+ DEFAULT -- 1
+ VALID VALUES -- [1,...]
+ SERVER DEFAULT PROPERTY -- min.insync.replicas
+ IMPORTANCE -- medium

**preallocate** -- Предварительное выделение файла на диске при создании нового сегмента журнала

+ TYPE -- boolean
+ DEFAULT -- false
+ SERVER DEFAULT PROPERTY -- log.preallocate
+ IMPORTANCE -- medium

**retention.bytes** -- Контроль максимального размера партиции (состоящей из сегментов журнала), который может увеличиваться до момента отказа от старых сегментов журнала с целью освобождения места при использовании политики хранения "delete". По умолчанию ограничения по размеру нет, есть только ограничение по времени. Поскольку данный предел применяется на уровне партиции, необходимо умножить значение лимита по времени на количество партиций, чтобы вычислить объем хранения топика в байтах

+ TYPE -- long
+ DEFAULT -- - 1
+ SERVER DEFAULT PROPERTY -- log.retention.bytes
+ IMPORTANCE -- medium

**retention.ms** -- Контроль максимального времени, в течение которого хранится журнал, прежде чем отбрасываются старые сегменты журнала с целью освобождения места при использовании политики хранения "delete". Параметр представляет собой SLA о том, как скоро потребители должны читать свои данные. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 604800000
+ SERVER DEFAULT PROPERTY -- log.retention.ms
+ IMPORTANCE -- medium

**segment.bytes** -- Контроль размера файла сегмента для журнала. Сохранение и очистка файла всегда выполняются единовременно, поэтому больший размер сегмента означает меньшее количество файлов, но при этом менее гранулированный контроль над хранением

+ TYPE -- int
+ DEFAULT -- 1073741824
+ VALID VALUES -- [14,...]
+ SERVER DEFAULT PROPERTY -- log.segment.bytes
+ IMPORTANCE -- medium

**segment.index.bytes** -- Контроль размера индекса, который отображает смещения в позициях файла. Предварительно индексный файл выделяется и сокращается только после сжатия журнала. Обычно параметр не требует изменений

+ TYPE -- int
+ DEFAULT -- 10485760
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.index.size.max.bytes
+ IMPORTANCE -- medium

**segment.jitter.ms** -- Максимальный рандомный джиттер. Вычитается из запланированного времени сжатия сегмента во избежание проблемы сегментации thundering herd (огромное количество процессов, ждущих события, в то время как требуется только один процесс). Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 0
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.roll.jitter.ms
+ IMPORTANCE -- medium

**segment.ms** -- Период времени, после которого ADS выполняет сжатие журнала, даже если файл сегмента не заполнен, с целью обеспечения сохранения или сжатия устаревших данных. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 604800000
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.roll.ms
+ IMPORTANCE -- medium

**unclean.leader.election.enable** -- Указывает, следует ли включить не входящие в набор ISR реплики и установка последнего средства в качестве лидера, даже если это может привести к потере данных

+ TYPE -- boolean
+ DEFAULT -- false
+ SERVER DEFAULT PROPERTY -- unclean.leader.election.enable
+ IMPORTANCE -- medium

