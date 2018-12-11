Настройка брокера
===================

Все настройки брокера хранятся в конфигурационном файле */etc/kafka/conf/server.properties*.

Основные конфигурации брокера:

+ *broker.id*
+ *log.dirs*
+ *zookeeper.connect*

Далее приведен список настроек с описанием и указанием их типа, значений по умолчанию и действительных, их важностью и режимом обновления.

**zookeeper.connect** -- Строка хоста Zookeeper

+ TYPE -- string
+ DEFAULT -- high
+ DYNAMIC UPDATE MODE -- read-only

**advertised.host.name** -- Применяется при неустановленном параметре *advertised.listeners* или *listeners*. Рекомендуется использовать *advertized.listeners*. Обозначает имя хоста для публикации в ZooKeeper для использования клиентами. В средах IaaS может отличаться от интерфейса, к которому привязывается брокер. Если параметр не задан, используется настроенное значение для *host.name*. В противном случае -- значение из *java.net.InetAddress.getCanonicalHostName()*

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**advertised.listeners** -- Слушатели для публикации в ZooKeeper для использования клиентами (если есть отличие от свойства *listeners*). В средах IaaS может отличаться от интерфейса, к которому привязывается брокер. Если параметр не задан, используется значение для *listeners*. В отличие от *listeners* значение "0.0.0.0" недопустимо

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- per-broker

**advertised.port** -- Применяется при неустановленном параметре *advertised.listeners* или *listeners*. Рекомендуется использовать *advertized.listeners*. Обозначает имя хоста для публикации в ZooKeeper для использования клиентами. В средах IaaS может отличаться от интерфейса, к которому привязывается брокер. Если параметр не задан, публикуется тот же порт, к которому привязан брокер

+ TYPE -- int
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**auto.create.topics.enable** -- Включение автоматического создания топика на сервере

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**auto.leader.rebalance.enable** -- Включение автоматической балансировки лидера. Балансировка лидера в фоновом режиме через регулярные промежутки времени

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only 

**background.threads** -- Количество потоков для различных задач фоновой обработки

+ TYPE -- int
+ DEFAULT -- 10
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**broker.id** -- Идентификатор брокера для сервера. Если значение не установлено, создается уникальный идентификатор брокера. Чтобы избежать конфликтов между id брокера, созданными с помощью zookeeper, и id брокера, настроенными пользователем, генерация идентификаторов брокера начинается с *reserved.broker.max.id + 1*

+ TYPE -- int
+ DEFAULT -- - 1
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**compression.type** -- Конечный тип сжатия для топика. Конфигурация принимает стандартные кодеки сжатия ("gzip", "snappy", "lz4"). Так же возможно "uncompressed", что эквивалентно отсутствию сжатия; и "producer", что означает сохранение исходного кодека сжатия, установленного поставщиком

+ TYPE -- string
+ DEFAULT -- producer
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**delete.topic.enable** -- Включение возможности удаления топика. При отключенном параметре удаление топика через администратора не имеет результата

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**host.name** -- Применяется только когда параметр *listeners* не установлен. Рекомендуется использовать *listeners*. Обозначает имя хоста брокера. Если параметр задан, то привязка выполняется только к данному адресу. Если параметр не задан, привязка выполняется ко всем интерфейсам

+ TYPE -- string
+ DEFAULT -- "" 
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**leader.imbalance.check.interval.seconds** -- Частота, с которой контроллер запускает проверку балансировки партиции

+ TYPE -- long
+ DEFAULT -- 300
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**leader.imbalance.per.broker.percentage** -- Коэффициент дисбаланса лидера, допустимый для каждого брокера. Контроллер запускает балансировку лидера, если он превышает данное значение для брокера. Указывается в процентах

+ TYPE -- int
+ DEFAULT -- 10
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**listeners** -- Listener List -- Разделенный запятыми список URI, которые прослушиваются, и имена слушателей сети. Если имя слушателя не является протоколом безопасности, необходимо установить *listener.security.protocol.map*. Для привязки ко всем интерфейсам указать имя хоста "0.0.0.0". Если имя хоста не указано, привязка осуществляется к интерфейсу по умолчанию. Примеры списков слушателей сети: PLAINTEXT://myhost:9092,SSL://:9091,CLIENT://0.0.0.0:9092,REPLICATION://localhost:9093

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- per-broker

**log.dir** -- Каталог хранения данных журнала (дополнительный для свойства log.dirs)

+ TYPE -- string
+ DEFAULT -- /tmp/kafka-logs
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.dirs** -- Каталоги хранения данных журнала. Если параметр не установлен, используется значение свойства *log.dir*

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- high

**log.flush.interval.messages** -- Количество накопленных в партиции журнала данных перед их сбросом на диск

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.flush.interval.ms** -- Максимальное время хранения данных в любом топике в памяти до их сброса на диск. Указывается в миллисекундах. Если параметр не установлен, используется значение *log.flush.scheduler.interval.ms*

+ TYPE -- long
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.flush.offset.checkpoint.interval.ms** -- Частота обновления постоянной записи последнего сброса, который действует как точка восстановления журнала

+ TYPE -- int
+ DEFAULT -- 60000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.flush.scheduler.interval.ms** -- Частота log flusher проверки на необходимость сброса какого-либо журнала на диск. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.flush.start.offset.checkpoint.interval.ms** -- Частота обновления постоянной записи смещения начала журнала

+ TYPE -- int
+ DEFAULT -- 60000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.retention.bytes** -- Максимальный размер журнала перед его удалением

+ TYPE -- long
+ DEFAULT -- - 1
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.retention.hours** -- Количество часов для хранения файла журнала перед его удалением, третично по отношению к свойству *log.retention.ms*. Указывается в часах

+ TYPE -- int
+ DEFAULT -- 168
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.retention.minutes** -- Количество минут для хранения файла журнала перед его удалением, вторично по отношению к свойству *log.retention.hours*. Указывается в минутах

+ TYPE -- int
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.retention.ms** -- Количество миллисекунд для хранения файла журнала перед его удалением. Указывается в миллисекундах. Если параметр не установлен, используется значение *log.retention.minutes*

+ TYPE -- long
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.roll.hours** -- Максимальное время до развертывания нового сегмента журнала, вторично по отношению к свойству *log.roll.ms*. Указывается в часах

+ TYPE -- int	
+ DEFAULT -- 168
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- [1,...]
+ DYNAMIC UPDATE MODE -- read-only

**log.roll.jitter.hours** -- Максимально допустимое значение джиттера для вычитания из *logRollTimeMillis*, вторично по отношению к свойству *log.roll.jitter.ms*. Указывается в часах

+ TYPE -- int
+ DEFAULT -- int
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.roll.jitter.ms** -- Максимально допустимое значение джиттера для вычитания из *logRollTimeMillis*. Указывается в миллисекундах. Если параметр не установлен, используется значение *log.roll.jitter.hours*

+ TYPE -- long
+ DEFAULT -- long
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.roll.ms** -- Максимальное время до развертывания нового сегмента журнала. Указывается в миллисекундах. Если параметр не установлен, используется значение *log.roll.hours* 

+ TYPE -- long
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.segment.bytes** -- Максимальный размер одного файла журнала

+ TYPE -- int
+ DEFAULT -- 1073741824
+ VALID VALUES -- [14,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.segment.delete.delay.ms** -- Время ожидания перед удалением файла из файловой системы

+ TYPE -- long
+ DEFAULT -- 60000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**message.max.bytes** -- Наибольший размер пакета данных, разрешенный ADS. При увеличении параметра следует также увеличить размер выборки для потребителей с целью обеспечения возможности получения пакета данных установленного размера. Параметр можно настроить для каждого топика с помощью поуровневой конфирурации топика *max.message.bytes*

+ TYPE -- int
+ DEFAULT -- 1000012
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**min.insync.replicas** -- При установленном поставщиком подтверждении acks на "all" или "-1", *min.insync.replicas* задается на минимальное количество реплик для подтверждения записи. Если этот минимум не может быть удовлетворен, то поставщик задает исключение (либо *NotEnoughReplicas*, либо *NotEnoughReplicasAfterAppend*). Совместное использование *min.insync.replicas* и acks обеспечивает более высокую гарантию к устойчивости. Типичным сценарием является создание топика с коэффициентом репликации *3*, параметром *min.insync.replicas* равным *2* и acks установленным на "all". Это гарантирует, что поставщик задает исключение, если большинство реплик не принимает запись

+ TYPE -- int
+ DEFAULT -- 1
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**num.io.threads** -- Число потоков, используемых сервером для обработки запросов, которые могут включать дисковые операции ввода-вывода

+ TYPE -- int
+ DEFAULT -- 8
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**num.network.threads** -- Количество потоков, используемых сервером для получения запросов от сети и отправки ответов в сеть

+ TYPE -- int
+ DEFAULT -- 3
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**num.recovery.threads.per.data.dir** -- Число потоков в каталоге данных, используемых для восстановления журнала при запуске или при сбросе по прекращению работы

+ TYPE -- int
+ DEFAULT -- 1
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**num.replica.alter.log.dirs.threads** -- Число потоков, которые могут перемещать реплики между каталогами журналов, включая дисковые операции ввода-вывода

+ TYPE -- int
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**num.replica.fetchers** -- Количество потоков выборки, используемых для репликации данных от исходного брокера. Увеличение этого значения может увеличить степень параллелизма ввода-вывода в брокере-подписчике

+ TYPE -- int
+ DEFAULT -- 1
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**offset.metadata.max.bytes** -- Максимальный размер для записи метаданных с учетом фиксации смещения

+ TYPE -- int
+ DEFAULT -- 4096
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.commit.required.acks** -- Принятие необходимых подтверждений acks перед фиксацией данных. Значение по умолчанию "-1" не следует переопределять

+ TYPE -- short
+ DEFAULT -- - 1
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.commit.timeout.ms** -- Фиксация смещения откладывается до тех пор, пока все реплики для топика смещения не получат коммит или данный установленный таймаут не будет достигнут. Аналогично времени ожидания запроса поставщика

+ TYPE -- int
+ DEFAULT -- 5000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.load.buffer.size** -- Размер пакета для чтения из сегментов смещений при загрузке смещений в кэш

+ TYPE -- int
+ DEFAULT -- 5242880
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.retention.check.interval.ms** -- Частота проверки устаревших смещений

+ TYPE -- long
+ DEFAULT -- 600000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.retention.minutes** -- Сброс смещений старше установленного срока хранения

+ TYPE -- int
+ DEFAULT -- 1440
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only 

**offsets.topic.compression.codec** -- Кодек сжатия для топика смещения. Сжатие может использоваться для достижения "атомных" коммитов

+ TYPE -- int
+ DEFAULT -- 0
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.topic.num.partitions** -- Количество партиций для коммита топика смещения (не следует изменять после развертывания)

+ TYPE -- int
+ DEFAULT -- 50
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.topic.replication.factor** -- Коэффициент репликации для топика смещения (устанавливается выше с целью обеспечения доступности). Создание внутреннего топика невозможно, пока размер кластера не соответствует данному требованию коэффициента репликации

+ TYPE -- short
+ DEFAULT -- 3
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.topic.segment.bytes** -- Размер сегмента топика смещений в байтах. Значение должно быть относительно небольшим с целью ускорения сжатия журнала и загрузку кэша

+ TYPE -- int
+ DEFAULT -- 104857600
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**port** -- Применяется при неустановленном параметре *listeners*. Рекомендуется использовать *listeners*. Обозначает порт для прослушивания и приема подключений

+ TYPE -- int
+ DEFAULT -- 9092
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**queued.max.requests** -- Количество запросов в очереди до блокировки сетевых потоков

+ TYPE -- int
+ DEFAULT -- 500
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**quota.consumer.default** -- Применяется при неустановленном параметре динамических квот по умолчанию в Zookeeper. Любой потребитель группы customerId/consumer дросселируется при получении большего количества байтов, чем данное установленное значение в секунду

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**quota.producer.default** -- Применяется при неустановленном параметре динамических квот по умолчанию в Zookeeper. Любой поставщик с известным clientId дросселируется при получении большего количества байтов, чем данное установленное значение в секунду

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**replica.fetch.min.bytes** -- Минимальное количество байт, ожидаемое для каждого ответа на выборку. При недостаточном объеме срабатывает параметр *replicaMaxWaitTimeMs*

+ TYPE -- int
+ DEFAULT -- 1
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**replica.fetch.wait.max.ms** -- Максимальное время ожидания для каждого запроса на выборку с последующей публикацией реплик. Значение всегда должно быть меньше параметра *replica.lag.time.max.ms* для предотвращения частого сжатия ISR низкопроизводительных топиков

+ TYPE -- int
+ DEFAULT -- 500
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**replica.high.watermark.checkpoint.interval.ms** -- Верхний предел частоты сохранения на диск (Частота сохранения высокого водяного знака на диск)

+ TYPE -- long
+ DEFAULT -- 5000
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**replica.lag.time.max.ms** -- Удаление подписчика лидером из isr в случае, если подписчик не отправил ни одного запроса на выборку или не считал конечное смещение журнала лидеров

+ TYPE -- long
+ DEFAULT -- 10000
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**replica.socket.receive.buffer.bytes** -- Буфер приема сокетов для сетевых запросов

+ TYPE -- int
+ DEFAULT -- 65536
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**replica.socket.timeout.ms** -- Время ожидания сокета для сетевых запросов. Значение должно быть не менее установленного параметра *replica.fetch.wait.max.ms*

+ TYPE -- int
+ DEFAULT -- 30000
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**request.timeout.ms** -- Максимальное время ожидания клиентом ответа на запрос. Если ответ не получен до истечения установленного значения, клиент повторно отправляет запрос при необходимости

+ TYPE -- int
+ DEFAULT -- 30000
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**socket.receive.buffer.bytes** -- Буфер SO_RCVBUF сокета сервера сокетов. При значении параметра "-1" используется ОС по умолчанию

+ TYPE -- int
+ DEFAULT -- 102400
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**socket.request.max.bytes** -- Максимальное количество байт в запросе сокета

+ TYPE -- int
+ DEFAULT -- 104857600
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**socket.send.buffer.bytes** -- Буфер SO_SNDBUF сокета сервера сокетов. При значении параметра "-1" используется ОС по умолчанию

+ TYPE -- int
+ DEFAULT -- 102400
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**transaction.max.timeout.ms** -- Максимально допустимое время ожидания для транзакций. Если запрошенное клиентом время транзакции превышает установленное значение, тогда брокер выдает ошибку в *InitProducerIdRequest*. Это предотвращает чрезмерное превышение времени ожидания для клиента, которое может тормозить чтение данных потребителями из топиков, включенных в транзакцию

+ TYPE -- int
+ DEFAULT -- 900000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**transaction.state.log.load.buffer.size** -- Размер пакета для чтения из сегментов журнала транзакций при загрузке в кэш идентификаторов поставщиков и транзакций

+ TYPE -- int
+ DEFAULT -- 5242880
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- [1,...]
+ DYNAMIC UPDATE MODE -- read-only

**transaction.state.log.min.isr** -- Переопределение конфигурации *min.insync.replicas* для топика транзакции

+ TYPE -- int
+ DEFAULT -- 2
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**transaction.state.log.num.partitions** -- Количество партиций для топика транзакции (после развертывания параметр должен остаться неизменным)

+ TYPE -- int
+ DEFAULT -- 50
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**transaction.state.log.replication.factor** -- Коэффициент репликации для топика транзакции (задается выше для обеспечения доступности). Создание внутреннего топика завершится ошибкой, пока размер кластера не будет соответствовать данному требованию к фактору репликации

+ TYPE -- short
+ DEFAULT -- 3
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**transaction.state.log.segment.bytes** -- Байты сегмента топика транзакции должны быть относительно небольшими для ускорения сжатия журнала и загрузки кэша

+ TYPE -- int
+ DEFAULT -- 104857600
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**transactional.id.expiration.ms** -- Максимальное время ожидания для координатора транзакций прежде, чем предварительно истечет срок действия идентификатора транзакции поставщика без получения обновлений состояния транзакции. Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- 604800000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**unclean.leader.election.enable** -- Указывает, следует ли включить не входящие в набор ISR реплики и установка последнего средства в качестве лидера, даже если это может привести к потере данных

+ TYPE -- boolean
+ DEFAULT -- false
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**zookeeper.connection.timeout.ms** -- Максимальное время ожидания клиентом установки соединения с Zookeeper. Если параметр не задан, используется значение для *zookeeper.session.timeout.ms*. Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**zookeeper.max.in.flight.requests** -- Максимальное количество неподтвержденных запросов, отправленных клиентом в Zookeeper, перед блокировкой

+ TYPE -- int
+ DEFAULT -- 10
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**zookeeper.session.timeout.ms** -- Тайм-аут сеанса Zookeeper. Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- int
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**zookeeper.set.acl** -- Настройка клиента для использования безопасных списков управления доступом ACL

+ TYPE -- boolean
+ DEFAULT -- boolean
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**broker.id.generation.enable** -- Автоматическое создание идентификатора брокера на сервере. При включенном параметре значение, настроенное для *reserved.broker.max.id*, должно быть пересмотрено

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**broker.rack** -- Стойка брокера. Используется при назначении репликации в стойке для отказоустойчивости. Примеры: "RACK1", "us-east-1d"

+ TYPE -- string
+ DEFAULT -- string
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**connections.max.idle.ms** -- Время ожидания бездействующих соединений: потоки процессора сокета сервера закрывают соединения, которые простаивают больше установленного значения. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 600000
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**controlled.shutdown.enable** -- Включение контролируемого завершения работы сервера

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**controlled.shutdown.max.retries** -- Контролируемое выключение может завершиться ошибкой по нескольким причинам: параметр определяет количество повторных попыток подключения при возникновении таких сбоев

+ TYPE -- int
+ DEFAULT -- 3
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**controlled.shutdown.retry.backoff.ms** -- Перед каждой повторной попыткой подключения системе требуется время для восстановления состояния, вызвавшего предыдущий сбой (сбой контроллера, задержка реплики и т.д.). Параметр определяет время ожидания перед повторной попыткой. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 5000
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**controller.socket.timeout.ms** -- Время ожидания сокета для каналов контроллер-брокер. Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- 30000
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**default.replication.factor** -- Коэффициенты репликации по умолчанию для автоматически создаваемых топиков

+ TYPE -- int
+ DEFAULT -- 1
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**delegation.token.expiry.time.ms** -- Время действия токена перед его обновлением. Значение по умолчанию 1 день. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 86400000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**delegation.token.master.key** -- Мастер/секретный ключ для создания и проверки делегированных токенов. Один и тот же ключ должен быть настроен для всех брокеров. Если ключ не установлен или задана пустая строка, брокеры отключают поддержку делегированных токенов

+ TYPE -- password
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**delegation.token.max.lifetime.ms** -- Максимальный срок действия токена, по истечении которого он больше не может быть обновлен. Значение по умолчанию 7 дней. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 604800000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**delete.records.purgatory.purge.interval.requests** -- Интервал очистки записей на удаление. Значение указывается в количестве запросов

+ TYPE -- int
+ DEFAULT -- 1
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**fetch.purgatory.purge.interval.requests** -- Интервал очистки запросов выборки. Значение указывается в количестве запросов

+ TYPE -- int
+ DEFAULT -- 1000
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**group.initial.rebalance.delay.ms** -- Время, в течение которого координатор группы ожидает присоединения большего числа потребителей к новой группе перед выполнением первой перебалансировки. Более длительная задержка означает потенциально меньшее количество перебалансировок, но увеличивает время до начала обработки. Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- 3000
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**group.max.session.timeout.ms** -- Максимально допустимое время ожидания сессии для зарегистрированных потребителей. Более длительные тайм-ауты дают потребителям больше времени для обработки данных между heartbeat-сообщениями за счет большего времени для выявления сбоев. Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- 300000
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**group.min.session.timeout.ms** -- Минимально допустимое время ожидания сессии для зарегистрированных потребителей. Более короткие тайм-ауты приводят к более быстрому обнаружению сбоев за счет более частых heartbeat-сообщений, которые могут перегружать ресурсы брокера. Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- 6000
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**inter.broker.listener.name** -- Имя слушателя для связи между брокерами. Если параметр не задан, имя слушателя определяется свойством *security.inter.broker.protocol*. Одновременная установка параметров *inter.broker.listener.name* и *security.inter.broker.protocol* вызывает ошибку

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**inter.broker.protocol.version** -- Версия межброкерского протокола. Обычно параметр задается после обновления всех брокеров до новой версии. Пример некоторых допустимых значений: "0.8.0", "0.8.1", "0.8.1.1", "0.8.2", "0.8.2.0", "0.8.2.1", "0.9.0.0", "0.9.0.1". Необходимо проверить ApiVersion для полного списка

+ TYPE -- string
+ DEFAULT -- 1.1-IV0
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**log.cleaner.backoff.ms** -- Время спящего режима при отсутствии журналов для очистки. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 15000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.cleaner.dedupe.buffer.size** -- Общая память, используемая для дедупликации журнала во всех чистых потоках

+ TYPE -- long
+ DEFAULT -- 134217728
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.cleaner.delete.retention.ms** -- Длительность хранения удаленных записей. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 86400000
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.cleaner.enable** -- Включение процесса очистки журналов для запуска на сервере. Параметр должен быть включен, если используются какие-либо топики с помощью *cleanup.policy=compact*, включая топик внутренних смещений. Если параметр отключен, данные топики не сжимаются и постоянно растут в объеме

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**log.cleaner.io.buffer.load.factor** -- Коэффициент загрузки буфера дедуплирования журнала очистки -- процент заполнения буфера дедуплирования. Более высокое значение позволит очистить больше журнала, но приведет к большему количеству хэш-конфликтов

+ TYPE -- double
+ DEFAULT -- 0.9
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.cleaner.io.buffer.size** -- Общая память, используемая для ввода-вывода буферов журнала очистки через все чистые потоки

+ TYPE -- int
+ DEFAULT -- 524288
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.cleaner.io.max.bytes.per.second** -- Очистка журнала дросселируется таким образом, чтобы сумма операций чтения и записи была меньше установленного значения

+ TYPE -- double
+ DEFAULT -- 1.7976931348623157E308
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.cleaner.min.cleanable.ratio** -- Минимальное отношение грязного журнала к общему журналу для журнала, пригодного для очистки

+ TYPE -- double
+ DEFAULT -- 0.5
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.cleaner.min.compaction.lag.ms** -- Минимальное время, в течение которого сообщение остается несжатым в журнале. Применяется только для журналов с функцией сжатия. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 0
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.cleaner.threads** -- Количество фоновых потоков для очистки журнала 

+ TYPE -- int
+ DEFAULT -- 1
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.cleanup.policy** -- Политика очистки по умолчанию для сегментов, превышающих период хранения. Допустимые политики: "delete" и "compact"

+ TYPE -- list
+ DEFAULT -- delete
+ VALID VALUES -- [compact, delete]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.index.interval.bytes** -- Интервал добавления записи в индекс смещения

+ TYPE -- int
+ DEFAULT -- 4096
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.index.size.max.bytes** -- Максимальный размер индекса смещения. Указывается в байтах

+ TYPE -- int
+ DEFAULT -- 10485760
+ VALID VALUES -- [4,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.message.format.version** -- Версия формата сообщений, которую брокер использует для добавления данных в журналы. Значение должно быть действительным ApiVersion. Некоторые примеры: "0.8.2", "0.9.0.0", "0.10.0". Необходимо проверить ApiVersion для получения более подробной информации. Установив версию формата сообщений, пользователь подтверждает, что все существующие данные на диске меньше или равны указанной версии. Неправильное задание параметра приводит к тому, что потребители с более старыми версиями получают данные в нечитаемом формате

+ TYPE -- string
+ DEFAULT -- 1.1-IV0
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**log.message.timestamp.difference.max.ms** -- Максимальное допустимое различие между отметкой времени, когда брокер получает сообщение, и отметкой времени, указанной в сообщении. При *log.message.timestamp.type=CreateTime* сообщение отклоняется, если разница в отметке времени превышает указанный порог. Конфигурация игнорируется, если *log.message.timestamp.type=LogAppendTime*. Максимально допустимое различие временных отметок должно быть не больше, чем *log.retention.ms*. Указывается в миллисекундах 

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.message.timestamp.type** -- Определить, является ли отметка времени в сообщении временем создания сообщения или временем добавления журнала. Параметр может принимать значение "CreateTime" либо "LogAppendTime"

+ TYPE -- string
+ DEFAULT -- CreateTime
+ VALID VALUES -- [CreateTime, LogAppendTime]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.preallocate** -- Предварительное выделение файла при создании нового сегмента. При испольовании платформы ADS в Windows рекомендуется установить значение "true"

+ TYPE -- boolean
+ DEFAULT -- false
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.retention.check.interval.ms** -- Частота проверки журналом очистки на наличие какого-либо журнала на удаление. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 300000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**max.connections.per.ip** -- Максимальное количество подключений с каждого IP-адреса

+ TYPE -- int
+ DEFAULT -- 2147483647
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**max.connections.per.ip.overrides** -- Ip или hostname переопределяет максимальное количество подключений по умолчанию

+ TYPE -- string
+ DEFAULT -- ""
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**max.incremental.fetch.session.cache.slots** -- Максимальное количество сессий инкрементной выборки

+ TYPE -- int
+ DEFAULT -- 1000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**num.partitions** -- Число партиций по умолчанию для каждого топика

+ TYPE -- int
+ DEFAULT -- 1
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**password.encoder.old.secret** -- Старый секрет для кодирования динамически настроенных паролей. Установка параметра требуется только при обновлении секрета. Если параметр задан, все динамически закодированные пароли декодируются и перекодируются с помощью *password.encoder.secret* при запуске брокера

+ TYPE -- password
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**password.encoder.secret** -- Секрет для кодирования динамически настроенных паролей для данного брокера

+ TYPE -- password
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**principal.builder.class** -- Полное имя класса, реализующего интерфейс ADSPrincipalBuilder, который используется для создания объекта ADSPrincipal во время авторизации. Конфигурация также поддерживает устаревший интерфейс PrincipalBuilder, который ранее использовался для аутентификации клиентов по протоколу SSL. Если параметр не задан, действие по умолчанию зависит от используемого протокола безопасности. Для аутентификации SSL имя принципала отличается от имени из сертификата клиента, если он предоставлен; в противном случае, если аутентификация клиента не требуется, имя принципала задается "ANONYMOUS". Для аутентификации SASL принципал задается на основании правил, определенных в *sasl.kerberos.principal.to.local.rules* с использованием GSSAPI и идентификатора аутентификации SASL для других механизмов. Для PLAINTEXT имя принципала -- "ANONYMOUS"

+ TYPE -- class
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**producer.purgatory.purge.interval.requests** -- Интервал очистки запросов поставщика. Значение указывается в количестве запросов

+ TYPE -- int
+ DEFAULT -- 1000
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**queued.max.request.bytes** -- Разрешенное число байтов в очереди до того, как запросы не будут прочитаны

+ TYPE -- long
+ DEFAULT -- - 1
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**replica.fetch.backoff.ms** -- Длительность спящего режима при возникновении ошибки партиции. Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- 1000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**replica.fetch.max.bytes** -- Количество байтов сообщений, получаемых каждой партицией. Параметр не является абсолютным максимумом. Если первый пакет записей в первой непустой партиции выборки больше установленного значения, пакет данных все равно будет возвращен для обеспечения гарантии возможности выполнения. Максимальный размер пакета записей, принятый брокером, определяется через *message.max.bytes* (конфигурация брокера) или *max.message.bytes* (конфигурация топика)

+ TYPE -- int
+ DEFAULT -- 1048576
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**replica.fetch.response.max.bytes** -- Максимальное количество байтов, ожидаемое для полного ответа на выборку. Параметр не является абсолютным максимумом. Записи извлекаются пакетами, и если первый пакет записей в первой непустой партиции выборки больше установленного значения, пакет данных все равно будет возвращен для обеспечения гарантии возможности выполнения. Максимальный размер пакета записей, принятый брокером, определяется через *message.max.bytes* (конфигурация брокера) или *max.message.bytes* (конфигурация топика)

+ TYPE -- int
+ DEFAULT -- 10485760
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**reserved.broker.max.id** -- Максимальное число, которое можно использовать для broker.id

+ TYPE -- int
+ DEFAULT -- 1000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**sasl.enabled.mechanisms** -- Список механизмов SASL, включенных на сервере ADS. Список может содержать любой механизм, для которого обеспечивается безопасность. По умолчанию включен только GSSAPI

+ TYPE -- list
+ DEFAULT -- GSSAPI
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**sasl.jaas.config** -- Параметры контекста входа JAAS для соединений SSL в формате, используемом файлами конфигурации JAAS. Формат файла конфигурации JAAS описан по `ссылке <http://docs.oracle.com/javase/8/docs/technotes/guides/security/jgss/tutorials/LoginConfigFile.html>`_. Формат значения: "(=)*;"

+ TYPE -- password
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**sasl.kerberos.kinit.cmd** -- Путь команд Kerberos kinit

+ TYPE -- string
+ DEFAULT -- /usr/bin/kinit
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**sasl.kerberos.min.time.before.relogin** -- Время ожидания авторизации потока между попытками обновления

+ TYPE -- long
+ DEFAULT -- 60000
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**sasl.kerberos.principal.to.local.rules** -- Список правил для сопоставления имен принципалов с короткими именами (обычно с именами пользователей операционной системы). Правила оцениваются по порядку, и первое правило, совпадающее с именем принципала, используется для сопоставления его с коротким именем. Все последующие правила в списке игнорируются. По умолчанию имена принципалов формы {username}/{hostname}@{REALM} сопоставляются с именем {username}. Важно обратить внимание, что данная конфигурация игнорируется, если расширение ADSPrincipalBuilder обеспечивается настройкой *main.builder.class*

+ TYPE -- list
+ DEFAULT -- DEFAULT
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**sasl.kerberos.service.name** -- Имя принципала Kerberos, которое запускает ADS. Значение можно определить в конфигурации ADS JAAS либо в конфигурации ADS

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**sasl.kerberos.ticket.renew.jitter** -- Процент случайного джиттера по отношению к времени возобновления 

+ TYPE -- double
+ DEFAULT -- 0.05
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**sasl.kerberos.ticket.renew.window.factor** -- Время ожидания авторизации потока до тех пор, пока не будет достигнут указанный коэффициент времени от последнего обновления до истечения срока действия тикета, и попытка возобновления тикета за этот период времени

+ TYPE -- double
+ DEFAULT -- 0.8
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**sasl.mechanism.inter.broker.protocol** -- Механизм SASL для взаимодействия между брокерами. По умолчанию используется GSSAPI

+ TYPE -- string
+ DEFAULT -- GSSAPI
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**security.inter.broker.protocol** -- Протокол безопасности для связи между брокерами. Допустимые значения: "PLAINTEXT", "SSL", "SASL_PLAINTEXT", "SASL_SSL". Одновременная установка параметров *security.inter.broker.protocol* и *inter.broker.listener.name* вызывает ошибку

+ TYPE -- string
+ DEFAULT -- PLAINTEXT
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- read-only

**ssl.cipher.suites** -- Список наборов шифров. Именованная комбинация аутентификации, шифрования, MAC и ключей обмена алгоритма для согласования параметров безопасности для сетевого подключения с использованием протокола TLS или SSL. По умолчанию поддерживаются все доступные варианты шифрования

+ TYPE -- list
+ DEFAULT -- ""
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.client.auth** -- Конфигурация брокера ADS для запроса аутентификации клиента. Следующие настройки являются общими:

  + *ssl.client.auth=required* -- требование проверки подлинности клиента;
  + *ssl.client.auth=request* -- аутентификация клиента является необязательной;
  + *ssl.client.auth=none* -- аутентификация клиента не требуется

+ TYPE -- string
+ DEFAULT -- none
+ VALID VALUES -- [required, requested, none]
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.enabled.protocols** -- Список протоколов, включенных для соединений SSL

+ TYPE -- list
+ DEFAULT -- TLSv1.2,TLSv1.1,TLSv1
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.key.password** -- Пароль закрытого ключа в файле хранилища ключей. Необязательный параметр для клиента

+ TYPE -- password
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.keymanager.algorithm** -- Алгоритм службы управления ключами для SSL-соединений. Значением по умолчанию является алгоритм, настроенный для Java Virtual Machine

+ TYPE -- string
+ DEFAULT -- SunX509
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.keystore.location** -- Расположение файла хранилища ключей. Необязательный параметр для клиента, может использоваться для двусторонней аутентификации клиента

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.keystore.password** -- Пароль хранилища для файла хранения ключей. Необязательный параметр для клиента, требуется только при настройке *ssl.keystore.location*

+ TYPE -- password
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.keystore.type** -- Формат файла хранилища ключей. Необязательный параметр для клиента

+ TYPE -- string
+ DEFAULT -- JKS
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.protocol** -- Протокол SSL для генерации SSLContext. Значение по умолчанию -- "TLS", что подходит для большинства случаев. Допустимыми значениями в последних JVM являются "TLS", "TLSv1.1" и "TLSv1.2". Протоколы "SSL", "SSLv2" и "SSLv3" могут поддерживаться в более старых JVM, но их использование не рекомендуется из-за известных уязвимостей безопасности

+ TYPE -- string
+ DEFAULT -- TLS
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.provider** -- Имя поставщика безопасности для соединений SSL. Значением по умолчанию является поставщик безопасности по умолчанию для JVM

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.trustmanager.algorithm** -- Алгоритм доверенной службы управления ключами для SSL-соединений. Значением по умолчанию является алгоритм, настроенный для Java Virtual Machine

+ TYPE -- string
+ DEFAULT -- PKIX
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.truststore.location** -- Расположение файла хранилища trust store  

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.truststore.password** -- Пароль для файла хранилища trust store. При неустановленном пароле доступ к хранилищу есть, но осуществляется с отключенной проверкой надежности

+ TYPE -- password
+ DEFAULT -- null
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.truststore.type** -- Формат файла хранилища trust store

+ TYPE -- string
+ DEFAULT -- JKS
+ IMPORTANCE -- medium
+ DYNAMIC UPDATE MODE -- per-broker

**alter.config.policy.class.name** -- Класс политики изменяемых конфигураций для их валидации. Класс осуществляет интерфейс *org.apache.kafka.server.policy.AlterConfigPolicy*

+ TYPE -- class
+ DEFAULT -- null
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**alter.log.dirs.replication.quota.window.num** -- Количество выборок для сохранения в памяти для квот репликации изменяемых журналов

+ TYPE -- int
+ DEFAULT -- 11
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**alter.log.dirs.replication.quota.window.size.seconds** -- Временной интервал каждой выборки для квот репликации изменяемых журналов. Указывается в секундах

+ TYPE -- int
+ DEFAULT -- 1
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**authorizer.class.name** -- Класс используемой авторизации

+ TYPE -- string
+ DEFAULT -- ""
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**create.topic.policy.class.name** -- Создание класса политики топика для его валидации. Класс осуществляет интерфейс *org.apache.kafka.server.policy.CreateTopicPolicy*

+ TYPE -- class
+ DEFAULT -- null
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**delegation.token.expiry.check.interval.ms** -- Интервал сканирования для удаления делегированных токенов с истекшим сроком действия. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 3600000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**listener.security.protocol.map** -- Сопоставление имен слушателей и протоколов безопасности. Параметр должен быть определен для того, чтобы один и тот же протокол безопасности мог использоваться в нескольких портах или IP-адресах. Например, внутренний и внешний трафик могут быть разделены, даже если для обоих требуется SSL. То есть, пользователь может определить слушателей с именами "INTERNAL" и "EXTERNAL" свойством: "INTERNAL:SSL, EXTERNAL:SSL", где ключ и значение разделяются двоеточием, а записи карты разделяются запятыми (без пробелов). Каждое имя слушателя должно отображаться на карте только один раз. Различные настройки безопасности (SSL и SASL) могут быть настроены для каждого слушателя путем добавления стандартизированного префикса (имя слушателя в нижнем регистре) к имени конфигурации. Например, чтобы установить другое хранилище ключей для внутреннего слушателя, будет установлена конфигурация с именем *listener.name.internal.ssl.keystore.location*. Если конфигурация для имени слушателя не задана, используется общая конфигурация (то есть *ssl.keystore.location*)

+ TYPE -- string
+ DEFAULT -- PLAINTEXT:PLAINTEXT,SSL:SSL,SASL_PLAINTEXT:SASL_PLAINTEXT,SASL_SSL:SASL_SSL
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- per-broker

**metric.reporters** -- Список классов для использования в качестве репортеров метрик. Реализация интерфейса *org.apache.kafka.common.metrics.MetricsReporter* позволяет подключать классы, которые будут уведомлены о создании новой метрики. JmxReporter всегда включен в реестр статистических данных JMX

+ TYPE -- list
+ DEFAULT -- ""
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- cluster-wide

**metrics.num.samples** -- Количество выборок, поддерживаемых для вычисления метрик

+ TYPE -- int
+ DEFAULT -- 2
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**metrics.recording.level** -- Самый высокий уровень записи для метрик

+ TYPE -- string
+ DEFAULT -- INFO 
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**metrics.sample.window.ms** -- Время ожидания вычисления метрик выборки. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 30000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**password.encoder.cipher.algorithm** -- Алгоритм шифрования, используемый для кодирования динамически настроенных паролей

+ TYPE -- string
+ DEFAULT -- AES/CBC/PKCS5Padding
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**password.encoder.iterations** -- Число итераций для кодирования динамически настроенных паролей

+ TYPE -- int
+ DEFAULT -- 4096
+ VALID VALUES -- [1024,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**password.encoder.key.length** -- Длина ключа, используемая для кодирования динамически настроенных паролей

+ TYPE -- int
+ DEFAULT -- 128
+ VALID VALUES -- [8,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**password.encoder.keyfactory.algorithm** -- Алгоритм SecretKeyFactory, используемый для кодирования динамически настроенных паролей. По умолчанию используется "PBKDF2WithHmacSHA512", если имеется, и "PBKDF2WithHmacSHA1" в противном случае

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**quota.window.num** -- Количество выборок, сохраняемых в памяти для квот клиента

+ TYPE -- int
+ DEFAULT -- 11
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**quota.window.size.seconds** -- Временной интервал каждой выборки для квот клиента. Указывается в секундах

+ TYPE -- int
+ DEFAULT -- 1
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**replication.quota.window.num** -- Количество выборок, сохраняемых в памяти для квот репликации

+ TYPE -- int
+ DEFAULT -- 11
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**replication.quota.window.size.seconds** -- Временной интервал каждой выборки для квот репликации. Указывается в секундах

+ TYPE -- int
+ DEFAULT -- 1
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**ssl.endpoint.identification.algorithm** -- Алгоритм идентификации конечных точек для валидации имени хоста сервера с использованием сертификата сервера

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- per-broker

**ssl.secure.random.implementation** -- Реализация SecureRandom PRNG, используемая для операций шифрования SSL

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- per-broker

**transaction.abort.timed.out.transaction.cleanup.interval.ms** -- Интервал, в течение которого выполняются отложенные транзакции. Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- 60000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**transaction.remove.expired.transaction.cleanup.interval.ms** -- Интервал удаления транзакций, срок действия которых истекает по установленному параметру *transactional.id.expiration.ms passing*. Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- 3600000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

**zookeeper.sync.time.ms** -- Удаленность последователя Zookeeper от лидера Zookeeper. Указывается в миллисекундах

+ TYPE -- int
+ DEFAULT -- 2000
+ IMPORTANCE -- low
+ DYNAMIC UPDATE MODE -- read-only

Более подробную информацию о конфигурации брокера можно найти в классе scala *kafka.server.KafkaConfig*.
