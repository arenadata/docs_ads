Kafka Java Consumer
====================

Платформа **ADS** включает в себя Java consumer, поставляемый вместе с **Kafka**.

В документе представлен общий обзор работы потребителя, введение в параметры конфигурации для настройки и примеры из каждой клиентской библиотеки.


Концепция
------------

Consumer group -- группа потребителей, взаимодействующих для использования данных из топиков. Партиции в топиках делятся между потребителями в группе, и при изменениях в группе партиции перераспределяются таким образом, что каждый потребитель получает пропорциональную долю партиций. Такой процесс называется перебалансировкой группы (rebalancing the group).

Основное различие в управлении группами между старым "high-level" потребителем и новым заключается в том, что первый зависит от **ZooKeeper**, а второй использует групповой протокол, встроенный в саму **Kafka**. В данном протоколе один из брокеров назначается координатором группы и отвечает за управление ее потребителями и за назначение им партиций.

Координатор каждой группы выбирается из лидеров внутренних смещений *__consumer_offsets*. Обычно идентификатор группы хэшируется в одной из партиций топика, и лидер данной партиции выбирается в качестве координатора. Таким образом, управление группами потребителей разделяется примерно поровну между всеми брокерами в кластере, что позволяет масштабировать количество групп за счет увеличения числом брокеров.

Когда потребитель запускается, он находит координатора для своей группы и отправляет запрос на присоединение. При этом координатор начинает перебалансировку, что приводит к формированию новой группы.

Каждый участник в группе должен посылать heartbeat-сообщения координатору. В случае если до истечения настроенного тайм-аута сессии такового не получено, координатор исключает потребителя из группы и переназначает его партиции.

Управление смещением (Offset Management)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

После получения потребителем назначения от координатора необходимо выявить начальную позицию для каждой определенной партиции. Когда группа создается впервые, до того, как какие-либо сообщения были использованы, позиция устанавливается в соответствии с политикой сброса смещения (*auto.offset.reset*). Как правило, потребление начинается с самого раннего либо с самого позднего смещения.

Потребителю необходимо фиксировать свои смещения в партиции в соответствии с ходом прочтения сообщений. Поскольку, если потребитель выходит из строя или выключается, его партиции переназначаются другому участнику группы, который начинает потребление сообщений с последнего закоммиченного смещения. В случае аварийного завершения работы потребитиеля до того, как какое-либо его смещение зафиксировано, следующий потребитель использует политику сброса.

Политика фиксации смещения имеет ключевое значение для обеспечения необходимых приложению гарантий доставки сообщений. По умолчанию потребитель настроен на использование политики автоматического коммита, которая инициирует фиксацию с периодическим интервалом. Также потребителем поддерживается API, который можно использовать для ручного управления смещением. В примерах приведено несколько подробных случаев API-фиксации и обсуждение компромиссов с точки зрения производительности и надежности (`Примеры`_).

При записи во внешнюю систему позиция потребителя должна быть согласована с тем, что хранится в виде выходных данных. Именно поэтому потребитель хранит свое смещение в том же месте, где выходные данные. Например, Kafka Connect записывает данные в **HDFS** вместе со смещениями считываемых данных, что гарантирует обоюдное обновление данных и смещений. Аналогичная схема применяется для многих других систем данных, требующих более строгой семантики, и для которых сообщения не имеют первичного ключа для обеспечения дедупликации.

Так **Kafka** поддерживает обработку exactly-once в Kafka Streams, и поставщик или потребитель транзакций может использоваться для обеспечения доставки exactly-once при передаче и обработке данных между топиками **Kafka**. В противном случае **Kafka** гарантирует доставку at-least-once по умолчанию, но при этом можно реализовать доставку at-most-once, отключив повторные попытки для поставщика и зафиксировав смещения в потребителе перед обработкой пакета сообщений.


Конфигурация
-------------

Полный список параметров конфигурации доступен в документе `Настройки платформы Arenadata Streaming <../../AdminGuide/Admin_Kafka/index>`_. Но некоторые из ключевых параметров и их влияние на поведение потребителя описаны в текущей главе.

Базовая конфигурация (Core Configuration)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Единственная обязательная настройка -- это *bootstrap.servers*, но при этом должен быть установлен *client.id* для сопоставления запросов в брокере со сделавшим их инстансом клиента. Как правило, все потребители в одной группе используют один и тот же идентификатор клиента.

Конфигурация группы (Group Configuration)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Параметр *group.id* должен быть всегда настроен за исключением случаев, когда API используется просто по назначению и нет необходимости хранить смещения в **Kafka**.

Тайм-аутом сессии можно управлять в параметре *session.timeout.ms*. Значение по умолчанию установлено на *30 секунд*, но если приложению требуется больше времени для обработки сообщений, то для избежания чрезмерной перебалансировки значение параметра можно безопасно увеличить. Это в основном актуально при использовании Java consumer и обработке сообщений в одном потоке. В таком случае также можно регулировать параметр *max.poll.records* для настройки количества требуемых для обработки на каждой итерации цикла записей (более подробно вопрос рассмотрен в главе `Основные возможности`_). 

Основным недостатком применения долгого тайм-аута сессии является то, что координатору требуется больше времени для обнаружения сбоя инстанса потребителя, а это значит, что другому потребителю в группе требуется больше времени для передачи партиций. Но при этом в случае необходимости нормального выключения потребитель отправляет координатору явный запрос покинуть группу, который инициирует немедленную перебалансировку.

Другим параметром, влияющим на поведение перебалансировки, является *heartbeat.interval.ms*. Он контролирует, как часто потребитель должен отправлять heartbeats-сообщения координатору. Это также способ, когда необходимость перебалансировки определяется засчет потребителя, поэтому более короткий интервал heartbeats-сообщений обычно означает более быструю перебалансировку. Значение по умолчанию составляет *3 секунды*. Для больших групп целесообразно увеличить значение параметра.

Управление смещением (Offset Management)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Двумя основными параметрами, влияющими на управление смещением, являются автоматическая фиксация и политика сброса смещения. В первом случае при установленном по умолчанию параметре *enable.auto.commit* потребитель автоматически фиксирует смещения с заданным в *auto.commit.interval.ms* интервалом (по умолчанию -- *5 секунд*).

Второй параметр *auto.offset.reset* определяет поведение потребителя, когда нет зафиксированной позиции смещения (в случае, когда группа инициализируется впервые), или когда оно выходит за пределы диапазона. Можно установить сброс положения на самое раннее смещение -- *earliest*, либо на самое позднее -- *latest* (задано по умолчанию). Также можно выбрать значение *none* для самостоятельной установки начального смещения и ручной обработки ошибок вне диапазона.


Инициализация
---------------

Потребитель Java consumer создается с помощью стандартного файла свойств *Properties*:

  ::
  
   Properties config = new Properties();
   config.put("client.id", InetAddress.getLocalHost().getHostName());
   config.put("group.id", "foo");
   config.put("bootstrap.servers", "host1:9092,host2:9092");
   new KafkaConsumer<K, V>(config);

.. important:: Ошибки конфигурации приводят к возникновению исключения *KafkaException* из конструктора *KafkaConsumer*

Конфигурация **C/C++** (*librdkafka*) похожа, но при этом необходимо обрабатывать ошибки конфигурации непосредственно при настройке свойств:

  ::
  
   char hostname[128];
   char errstr[512];
   
   rd_kafka_conf_t *conf = rd_kafka_conf_new();
   
   if (gethostname(hostname, sizeof(hostname))) {
    fprintf(stderr, "%% Failed to lookup hostname\n");
    exit(1);
   }
   
   if (rd_kafka_conf_set(conf, "client.id", hostname,
                        errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK) {
    fprintf(stderr, "%% %s\n", errstr);
    exit(1);
   }
   
   if (rd_kafka_conf_set(conf, "group.id", "foo",
                        errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK) {
    fprintf(stderr, "%% %s\n", errstr);
    exit(1);
   }
   
   if (rd_kafka_conf_set(conf, "bootstrap.servers", "host1:9092,host2:9092",
                        errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK) {
    fprintf(stderr, "%% %s\n", errstr);
    exit(1);
   }
   
   /* Create Kafka consumer handle */
   rd_kafka_t *rk;
   if (!(rk = rd_kafka_new(RD_KAFKA_CONSUMER, conf,
                          errstr, sizeof(errstr)))) {
    fprintf(stderr, "%% Failed to create new consumer: %s\n", errstr);
    exit(1);
   }


Клиент **Python** может быть настроен через словарь следующим образом:

  ::
  
   from confluent_kafka import Consumer
   
   conf = {'bootstrap.servers': "host1:9092,host2:9092",
           'group.id': "foo",
           'default.topic.config': {'auto.offset.reset': 'smallest'}}
   
   consumer = Consumer(conf)


Клиент **Go** использует объект *ConfigMap* для передачи конфигурации потребителю:

  ::
  
   import (
       "github.com/confluentinc/confluent-kafka-go/kafka"
   )
   
   consumer, err := kafka.NewConsumer(&kafka.ConfigMap{
        "bootstrap.servers":    "host1:9092,host2:9092",
        "group.id":             "foo",
        "default.topic.config": kafka.ConfigMap{"auto.offset.reset": "smallest"}})


В **C#** испольуется *Dictionary<string, object>*:

  ::
  
   using System.Collections.Generic;
   using Confluent.Kafka;
   
   ...
   
   var config = new Dictionary<string, object>
   {
       { "bootstrap.servers", "host1:9092,host2:9092" },
       { "group.id", "foo" },
       { "default.topic.config", new Dictionary<string, object>
           {
               { "auto.offset.reset", "smallest" }
           }
       }
   }
   
   using (var consumer = new Consumer<Null, string>(config, null, new StringDeserializer(Encoding.UTF8)))
   {
       ...
   }



Основные возможности
----------------------

Хотя Java-клиент и *librdkafka* имеют много общих опций конфигурации и базовых функций, они используют довольно разные подходы, когда дело доходит до их потоковой модели и работы с потребителями. Прежде чем углубляться в примеры, полезно разобраться в дизайне API каждого клиента.


Java Client
^^^^^^^^^^^^^

**Java Client** разработан вокруг цикла обработки событий под управляением *poll()* API. Конструкция мотивирована системными вызовами UNIX *select* и *poll*. Базовый цикл потребления с Java API обычно принимает следующую форму:

  ::
  
   while (running) {
     ConsumerRecords<K, V> records = consumer.poll(Long.MAX_VALUE);
     process(records); // application-specific processing
     consumer.commitSync();
   }

В Java consumer нет фонового потока. API зависит от вызовов *poll()* для управления всеми операциями ввода-вывода, включая:

+ Присоединение к группе потребителей и обработка перебалансировкой партиций;
+ Периодичная отправка heartbeats-сообщений;
+ Периодичная отправка зафиксированных смещений (при включенном автокоммите);
+ Отправка и получение запросов на выборку для назначенных партиций.

Такая однопоточная модель означает, что нельзя отправлять heartbeats-сообщения, пока приложение обрабатывает записи по вызову *poll()*. Это приводит к тому, что потребитель выпадает из группы, если цикл обработки событий завершается, либо если задержка в обработке записи приводит к истечению времени ожидания сессии до следующей итерации цикла. Так и было задумано. Одна из проблем, которую пытается решить **Java Client**, -- обеспечение жизнедеятельности потребителей в группе. В то время, пока потребителю назначены партиции, другие члены группы не могут их же использовать, поэтому важно убедиться, что каждый конкретный потребитель действительно прогрессирует.

Данная функция защищает приложение от большого класса сбоев, но недостатком является то, что необходима настройка времени ожидания сессии так, чтобы потребитель не превышал его в своей обычной обработке записей. Кроме этого параметр *max.poll.records* устанавливает верхнюю границу количества записей, возвращаемых при каждом вызове. Поэтому важно использовать *poll()* и *max.poll.records* с достаточно большим тайм-аутом сессии (например, от *30* до *60 секунд*) и ограничивать количество обработанных записей на каждой итерации.

В случае если данные параметры не настроены надлежащим образом, это, как правило, приводит к исключению *CommitFailedException* смещения для обработанных записей. При использовании политики автоматической фиксации, можно даже не заметить, когда это происходит, так как потребитель молча игнорирует сбои коммитов (если только это не происходит достаточно часто, чтобы повлиять на показатели задержки). Исключение можно перехватить и либо проигнорировать, либо выполнить необходимую логику отката:

  ::

   while (running) {
     ConsumerRecords<K, V> records = consumer.poll(Long.MAX_VALUE);
     process(records); // application-specific processing
     try {
       consumer.commitSync();
     } catch (CommitFailedException e) {
       // application-specific rollback of processed records
     }
   }


C/C++ Client (librdkafka)
^^^^^^^^^^^^^^^^^^^^^^^^^^^

*Librdkafka* использует многопоточный подход к потреблению **Kafka**. С точки зрения пользователя, взаимодействие с API не слишком отличается от примера, используемого Java-клиентом, когда пользователь вызывает *rd_kafka_consumer_poll* в цикле, хотя данный API возвращает только одно сообщение или событие за раз:

  ::
  
   while (running) {
    rd_kafka_message_t *rkmessage = rd_kafka_consumer_poll(rk, 500);
    if (rkmessage) {
      msg_process(rkmessage);
      rd_kafka_message_destroy(rkmessage);
   
      if ((++msg_count % MIN_COMMIT_COUNT) == 0)
        rd_kafka_commit(rk, NULL, 0);
    }
   }


В отличие от Java-клиента, *librdkafka* выполняет всю выборку и координирует взаимодействие в фоновых потоках, что освобождает от сложности настройки тайм-аута сессии в соответствии с ожидаемым временем обработки. Однако, поскольку фоновый поток поддерживает потребителя до тех пор, пока клиент не закроется, важно убедиться, что процесс не простаивает, так как в этом случае он продолжает удерживать назначенные ему партиции.

При этом перебалансировка партиций также происходит в фоновом потоке, а это говорит о том, что все равно приходится обрабатывать потенциальные ошибки коммита, поскольку потребитель может больше не иметь того же назначения партиций, когда начинается фиксация. Это не требуется при включенном автокоммите, так как при этом ошибки коммита игнорируются в автоматическом режиме, но это также означает, что нет возможности откатить обработку.

  ::
  
   while (running) {
     rd_kafka_message_t *rkmessage = rd_kafka_consumer_poll(rk, 1000);
     if (!rkmessage)
       continue; // timeout: no message
   
     msg_process(rkmessage); // application-specific processing
     rd_kafka_message_destroy(rkmessage);
   
     if ((++msg_count % MIN_COMMIT_COUNT) == 0) {
       rd_kafka_resp_err_t err = rd_kafka_commit(rk, NULL, 0);
       if (err) {
         // application-specific rollback of processed records
       }
     }
   }


Python, Go и .NET Clients
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Клиенты **Python**, **Go** и **.NET** на внутреннем уровне используют *librdkafka*, поэтому у них также применяется многопоточный подход к потреблению **Kafka**. С точки зрения пользователя, взаимодействие с API не слишком отличается от примера, используемого Java-клиентом, когда пользователь вызывает метод *poll()* в цикле, хотя данный API возвращает только одно сообщение за раз.

**Python**:

  ::
  
   try:
       msg_count = 0
       while running:
           msg = consumer.poll(timeout=1.0)
           if msg is None: continue
   
           msg_process(msg) # application-specific processing
           msg_count += 1
           if msg_count % MIN_COMMIT_COUNT == 0:
               consumer.commit(async=False)
   finally:
       # Shut down consumer
       consumer.close()


**Go**:

  ::
  
   for run == true {
       ev := consumer.Poll(0)
       switch e := ev.(type) {
       case *kafka.Message:
           // application-specific processing
       case kafka.Error:
           fmt.Fprintf(os.Stderr, "%% Error: %v\n", e)
           run = false
       default:
           fmt.Printf("Ignored %v\n", e)
       }
   }


Поведение потребителя **C#** аналогично, за исключением того, что перед входом в цикл *Poll* необходимо настроить обработчики для различных типов событий, что эффективно делается внутри метода *Poll* (важно обратить внимание, что весь код выполняется в том же потоке):

  ::
  
   consumer.OnMessage += (_, msg) =>
   {
       // handle message.
   }
   
   consumer.OnPartitionEOF += (_, end)
       => Console.WriteLine($"Reached end of topic {end.Topic} partition {end.Partition}.");
   
   consumer.OnError += (_, error)
   {
       Console.WriteLine($"Error: {error}");
       cancelled = true;
   }
   
   while (!cancelled)
   {
       consumer.Poll(TimeSpan.FromSeconds(1));
   }



Примеры
---------

Далее приведены подробные примеры использования consumer API с особым вниманием к управлению смещением и семантике доставки. 


Basic Poll Loop
^^^^^^^^^^^^^^^^

API потребителя сосредоточен вокруг метода *poll()* для получения записей от брокеров и метода *subscribe()* для выбора топиков. Как правило, потребитель первоначально обращается к методу *subscribe()* для настройки интересующих топиков, а затем запускает цикл *poll()* до завершения работы приложения.

Потребитель намеренно избегает конкретной модели потоков, так как это не безопасно для многопоточного доступа и не дает возможности наличия собственных фоновых потоков. В частности, это означает, что все операции ввода-вывода происходят в потоке, вызыванном методом *poll()*. В приведенном ниже примере цикл опроса заключен в *Runnable*, который упрощает использование с *ExecutorService*:

  ::
  
   public abstract class BasicConsumeLoop implements Runnable {
     private final KafkaConsumer<K, V> consumer;
     private final List<String> topics;
     private final AtomicBoolean shutdown;
     private final CountDownLatch shutdownLatch;
   
     public BasicConsumeLoop(Properties config, List<String> topics) {
       this.consumer = new KafkaConsumer<>(config);
       this.topics = topics;
       this.shutdown = new AtomicBoolean(false);
       this.shutdownLatch = new CountDownLatch(1);
     }
   
     public abstract void process(ConsumerRecord<K, V> record);
   
     public void run() {
       try {
         consumer.subscribe(topics);
   
         while (!shutdown.get()) {
           ConsumerRecords<K, V> records = consumer.poll(500);
           records.forEach(record -> process(record));
         }
       } finally {
         consumer.close();
         shutdownLatch.countDown();
       }
     }
   
     public void shutdown() throws InterruptedException {
       shutdown.set(true);
       shutdownLatch.await();
     }
   }


В примере жестко запрограммировано время ожидания опроса на *500 миллисекунд*, то есть, если никаких записей не получено до истечения тайм-аута, *poll()* возвращает пустой набор записей. В случае если обработка сообщений связана с дополнительными затратами на настройку, можно добавить проверку ярлыков.

Для отключения потребителя добавляется флаг, который проверяется на каждой итерации цикла. При этом потребитель ожидает *500 миллисекунд* (плюс время обработки сообщения) перед завершением работы. Лучший подход представлен далее в примере.

Важно обратить внимание, что всегда следует вызывать *close()* после завершения работы потребителя. Это обеспечивает закрытие активных сокетов и очистку внутреннего состояния. Также это немедленно инициирует перебалансировку группы, что в свою очередь гарантирует переназначение всех принадлежащих данному потребителю партиций другому члену группы. Если не выполнить закрытие должным образом, брокер инициирует перебалансировку только после истечения времени ожидания сессии. В примере добавлена защелка (latch) для того, чтобы у потребителя было время завершить закрытие перед выключением.

Этот же пример выглядит аналогично в *librdkafka*:

  ::
  
   static int shutdown = 0;
   static void msg_process(rd_kafka_message_t message);
   
   void basic_consume_loop(rd_kafka_t *rk,
                           rd_kafka_topic_partition_list_t *topics) {
     rd_kafka_resp_err_t err;
   
     if ((err = rd_kafka_subscribe(rk, topics))) {
       fprintf(stderr, "%% Failed to start consuming topics: %s\n", rd_kafka_err2str(err));
       exit(1);
     }
   
     while (running) {
       rd_kafka_message_t *rkmessage = rd_kafka_consumer_poll(rk, 500);
       if (rkmessage) {
         msg_process(rkmessage);
         rd_kafka_message_destroy(rkmessage);
       }
     }
   
     err = rd_kafka_consumer_close(rk);
     if (err)
       fprintf(stderr, "%% Failed to close consumer: %s\n", rd_kafka_err2str(err));
     else
       fprintf(stderr, "%% Consumer closed\n");
   }


В **Python**:

  ::
  
   running = True
   
   def basic_consume_loop(consumer, topics):
       try:
           consumer.subscribe(topics)
   
           while running:
               msg = consumer.poll(timeout=1.0)
               if msg is None: continue
   
               if msg.error():
                   if msg.error().code() == KafkaError._PARTITION_EOF:
                       # End of partition event
                       sys.stderr.write('%% %s [%d] reached end at offset %d\n' %
                                        (msg.topic(), msg.partition(), msg.offset()))
                   elif msg.error():
                       raise KafkaException(msg.error())
               else:
                   msg_process(msg)
       finally:
           # Close down consumer to commit final offsets.
           consumer.close()
   
   def shutdown():
       running = False

В **Go**:

  ::
  
   err = consumer.SubscribeTopics(topics, nil)
   
   for run == true {
       ev := consumer.Poll(0)
       switch e := ev.(type) {
       case *kafka.Message:
           fmt.Printf("%% Message on %s:\n%s\n",
               e.TopicPartition, string(e.Value))
       case kafka.PartitionEOF:
           fmt.Printf("%% Reached %v\n", e)
       case kafka.Error:
           fmt.Fprintf(os.Stderr, "%% Error: %v\n", e)
           run = false
       default:
           fmt.Printf("Ignored %v\n", e)
       }
   }
   
   consumer.Close()

В **C#**:

  ::
  
   using (var consumer = new Consumer<Null, string>(config, null, new StringDeserializer(Encoding.UTF8)))
   {
       consumer.OnMessage += (_, msg)
           => Console.WriteLine($"Message value: {msg.Value}");
   
       consumer.OnPartitionEOF += (_, end)
           => Console.WriteLine($"Reached end of topic {end.Topic} partition {end.Partition}.");
   
       consumer.OnError += (_, error)
       {
           Console.WriteLine($"Error: {error}");
           cancelled = true;
       }
   
       consumer.Subscribe(topics);
   
       while (!cancelled)
       {
           consumer.Poll(TimeSpan.FromSeconds(1));
       }
   }


Хотя API-интерфейсы схожи, клиенты **C/C++**, **Python**, **Go** и **C#** используют другой подход, нежели **Java**. В то время как потребитель **Java** выполняет все операции ввода-вывода и обработку в потоке переднего плана (foreground thread), остальные клиенты используют фоновый поток (background thread). Основным следствием использования многопоточности (multiple threads) является то, что вызов *rd_kafka_consumer_poll* или *Consumer.poll()* абсолютно безопасен, то есть можно распараллеливать обработку сообщений по нескольким потокам. С высокого уровня опрос извлекает сообщения из очереди, которая заполняется в фоновом потоке.

Другим следствием использования фонового потока является то, что в нем выполняются все heartbeats-сообщения и перебалансировки. Преимущество заключается в отсутствии беспокойства об обработке сообщений, которая может стать следствием "пропуска" потребителем перебалансировки. Недостатком является то, что фоновый поток продолжает отправку heartbeats-сообщений, даже если процессор сообщений умер. А в таком случае потребитель удерживает свои партиции, и задержка чтения продолжается до выключения процесса.

Хотя клиенты используют различные подходы, они не так далеки друг от друга, как кажется. Для обеспечения той же абстракции в клиенте **Java** можно поместить очередь между циклом опроса и процессором сообщений, тогда poll loop заполняет очередь, а процессоры по ней извлекают сообщения.


Shutdown и Wakeup
^^^^^^^^^^^^^^^^^^

Альтернативным шаблоном для цикла опроса в Java-клиенте является использование *Long.MAX_VALUE* для тайм-аута. Для выхода из цикла можно использовать метод потребителя *wakeup()* из отдельного потока. Это вызывает исключение *WakeupException* из потока, блокирующего *poll()*. Если поток блокируется некорректно, то это приводит к вызову следующего опроса.

  ::
  
   public abstract class ConsumeLoop implements Runnable {
     private final KafkaConsumer<K, V> consumer;
     private final List<String> topics;
     private final CountDownLatch shutdownLatch;
   
     public BasicConsumeLoop(KafkaConsumer<K, V> consumer, List<String> topics) {
       this.consumer = consumer;
       this.topics = topics;
       this.shutdownLatch = new CountDownLatch(1);
     }
   
     public abstract void process(ConsumerRecord<K, V> record);
   
     public void run() {
       try {
         consumer.subscribe(topics);
   
         while (true) {
           ConsumerRecords<K, V> records = consumer.poll(Long.MAX_VALUE);
           records.forEach(record -> process(record));
         }
       } catch (WakeupException e) {
         // ignore, we're closing
       } catch (Exception e) {
         log.error("Unexpected error", e);
       } finally {
         consumer.close();
         shutdownLatch.countDown();
       }
     }
   
     public void shutdown() throws InterruptedException {
       consumer.wakeup();
       shutdownLatch.await();
     }
   }


Синхронные коммиты
^^^^^^^^^^^^^^^^^^^^

В предыдущих примерах предполагается, что потребитель настроен на автоматическую фиксацию смещений (по умолчанию). Auto-commit в основном работает как cron с периодом, установленным через свойство конфигурации *auto.commit.interval.ms*. Если потребитель аварийно завершает работу, то после перезапуска или перебалансировки положение всех принадлежащих ему партиций сбрасывается до последнего зафиксированного смещения. При этом последний коммит может быть таким же старым, как и сам интервал автоматической фиксации. Любые сообщения, поступившие с момента последнего коммита, необходимо прочитать повторно.

Очевидно, что для сокращения окна дубликатов можно уменьшить интервал автоматической фиксации, но некоторым пользователям может потребоваться еще более точный контроль над смещениями. Поэтому потребитель поддерживает commit API, который дает полный контроль над смещениями. Самый простой и надежный способ ручной фиксации смещений -- использовать синхронную фиксацию с помощью *commitSync()*, вызов которой блокирует поток до успешно выполненного коммита.

При непосредственном использовании API фиксации необходимо сначала отключить автоматический коммит в конфигурации, установив для свойства *enable.auto.commit* значение *false*.

  ::
  
   private void doCommitSync() {
     try {
       consumer.commitSync();
     } catch (WakeupException e) {
       // we're shutting down, but finish the commit first and then
       // rethrow the exception so that the main loop can exit
       doCommitSync();
       throw e;
     } catch (CommitFailedException e) {
       // the commit failed with an unrecoverable error. if there is any
       // internal state which depended on the commit, you can clean it
       // up here. otherwise it's reasonable to ignore the error and go on
       log.debug("Commit failed", e);
     }
   }
   
   public void run() {
     try {
       consumer.subscribe(topics);
   
       while (true) {
         ConsumerRecords<K, V> records = consumer.poll(Long.MAX_VALUE);
         records.forEach(record -> process(record));
         doCommitSync();
       }
     } catch (WakeupException e) {
       // ignore, we're closing
     } catch (Exception e) {
       log.error("Unexpected error", e);
     } finally {
       consumer.close();
       shutdownLatch.countDown();
     }
   }


В данном примере блок *try/catch* добавлен к вызову *commitSync*. Когда фиксация не может быть завершена по причине перебалансировки группы, выдается *CommitFailedException*. Это главное, что с осторожностью необходимо соблюдать при использовании клиента **Java**. Поскольку все сетевые операции ввода-вывода (включая heartbeats-сообщения) и обработка сообщений выполняются в потоке переднего плана, тайм-аут сессии может истечь во время обработки пакета сообщений. Чтобы справиться с этим, есть два варианта.

В первом варианте сначала можно настроить параметр *session.timeout.ms*, чтобы у обработчика было достаточно времени для завершения обработки сообщений. Затем можно настроить *max.partition.fetch.bytes*, чтобы ограничить объем данных, возвращаемых в одном пакете, но при этом приходится учитывать, сколько партиций содержится в подписанных топиках.

Второй вариант заключается в обработке сообщений в отдельном потоке, но тогда приходится управлять потоком передачи данных, чтобы потоки не отставали. Например, простого помещения сообщений в очередь блокировки, вероятно, недостаточно, если скорость обработки не поспевает за скоростью доставки (в этом случае может не понадобиться отдельный поток). Это может даже усугубить проблему, если цикл опроса заблокирован при вызове метода *offer()*, так как тогда фоновый поток обрабатывает еще больший пакет сообщений. API **Java** предлагает метод *pause()*, чтобы помочь в подобных ситуациях.

В данном случае необходимо установить *session.timeout.ms* достаточно большим, чтобы сбои при перебалансировках происходили реже. Как упомянуто выше, единственным недостатком этого является более длительная задержка переназначения партиций в случае серьезного сбоя (когда потребитель не может быть чисто завершен с помощью *close()*), что на практике редко происходит.

Важно проявить осторожность, так как функция *wakeup()* может быть запущена, пока коммит находится в состоянии ожидания. Рекурсивный вызов безопасней, поскольку инициирует wakeup только один раз.

В **C/C++** (*librdkafka*) можно получить похожее поведение с *rd_kafka_commit*, который используется как для синхронных, так и для асинхронных фиксаций. Однако подход немного отличается, поскольку *rd_kafka_consumer_poll* возвращает отдельные сообщения вместо пакетов, как это делает потребитель **Java**.

  ::
  
   void consume_loop(rd_kafka_t *rk,
                     rd_kafka_topic_partition_list_t *topics) {
     static const int MIN_COMMIT_COUNT = 1000;
   
     int msg_count = 0;
     rd_kafka_resp_err_t err;
   
     if ((err = rd_kafka_subscribe(rk, topics))) {
       fprintf(stderr, "%% Failed to start consuming topics: %s\n", rd_kafka_err2str(err));
       exit(1);
     }
   
     while (running) {
       rd_kafka_message_t *rkmessage = rd_kafka_consumer_poll(rk, 500);
       if (rkmessage) {
         msg_process(rkmessage);
         rd_kafka_message_destroy(rkmessage);
   
         if ((++msg_count % MIN_COMMIT_COUNT) == 0)
           rd_kafka_commit(rk, NULL, 0);
       }
    }
   
     err = rd_kafka_consumer_close(rk);
     if (err)
       fprintf(stderr, "%% Failed to close consumer: %s\n", rd_kafka_err2str(err));
     else
       fprintf(stderr, "%% Consumer closed\n");
   }


В данном примере синхронная фиксация запускается каждые *1000* сообщений. Вторым аргументом *rd_kafka_commit* является список смещений, которые должны быть зафиксированы; при значении *NULL* *librdkafka* фиксирует последние смещения для назначенных позиций. Третий аргумент в *rd_kafka_commit* -- флаг, который определяет асинхронность вызова. Коммит также можно активировать по истечению тайм-аута, чтобы убедиться, что зафиксированная позиция регулярно обновляется.

Поскольку клиент **Python** внутренне использует *librdkafka*, он применяет аналогичный шаблон, устанавливая параметр *async* для вызова метода *Consumer.commit()*. Этот метод также может принимать взаимоисключающие смещения параметров ключевых слов для явного перечисления смещений каждой назначенной партиции топика и *message*, которые фиксируют смещения относительно объекта *Message*, возвращаемого функцией *poll()*.

  ::
  
   def consume_loop(consumer, topics):
       try:
           consumer.subscribe(topics)
   
           msg_count = 0
           while running:
               msg = consumer.poll(timeout=1.0)
               if msg is None: continue
   
               if msg.error():
                   if msg.error().code() == KafkaError._PARTITION_EOF:
                       # End of partition event
                       sys.stderr.write('%% %s [%d] reached end at offset %d\n' %
                                        (msg.topic(), msg.partition(), msg.offset()))
                   elif msg.error():
                       raise KafkaException(msg.error())
               else:
                   msg_process(msg)
                   msg_count += 1
                   if msg_count % MIN_COMMIT_COUNT == 0:
                       consumer.commit(async=False)
       finally:
           # Close down consumer to commit final offsets.
           consumer.close()

Клиент **Go** также внутренне использует *librdkafka*, поэтому он применяет похожий шаблон, но обеспечивает при этом только синхронный вызов метода *Commit()*. Другие варианты методов фиксации также принимают список смещений для коммитов или *Message*, чтобы зафиксировать смещения относительно считываемого сообщения. При использовании ручного коммита важно отключить конфигурацию *enable.auto.commit*.

  ::
  
   msg_count := 0
   for run == true {
       ev := consumer.Poll(0)
       switch e := ev.(type) {
       case *kafka.Message:
           msg_count += 1
           if msg_count % MIN_COMMIT_COUNT == 0 {
               consumer.Commit()
           }
           fmt.Printf("%% Message on %s:\n%s\n",
               e.TopicPartition, string(e.Value))
   
       case kafka.PartitionEOF:
           fmt.Printf("%% Reached %v\n", e)
       case kafka.Error:
           fmt.Fprintf(os.Stderr, "%% Error: %v\n", e)
           run = false
       default:
           fmt.Printf("Ignored %v\n", e)
       }
   }


Клиент **C#** обеспечивает метод *CommitAsync* с возможными перегрузками. Его можно использовать синхронно, отвечая *Result* или *Wait()* на возвращаемый *Task*. Существуют варианты, которые фиксируют все смещения в текущем назначении, конкретный список смещений или смещение на основе *Message*.

  ::
  
   var msgCount = 0;
   
   consumer.OnMessage += (_, msg) =>
   {
       msgCount += 1;
       if (msgCount % MIN_COMMIT_COUNT == 0)
       {
           consumer.CommitAsync().Wait();
       }
       Console.WriteLine($"Message value: {msg.Value}");
   }
   
   consumer.OnPartitionEOF += (_, end)
       => Console.WriteLine($"Reached end of topic {end.Topic} partition {end.Partition}.");
   
   consumer.OnError += (_, error)
   {
       Console.WriteLine($"Error: {error}");
       cancelled = true;
   }
   
   while (!cancelled)
   {
       consumer.Poll(TimeSpan.FromSeconds(1));
   }


Использование автоматической фиксации обеспечивает доставку "at least once": **Kafka** гарантирует, что ни одно сообщение не будет пропущено, но возможны дубликаты. В предыдущем примере обеспечивается такая доставка, поскольку фиксация следует за обработкой сообщения. Однако, изменив запрос, можно получить доставку "at most once". Но при этом следует быть осторожнее с ошибкой коммита, для этого необходимо изменить *doCommitSync*, чтобы он возвращал информацию об успешности транзакции. Так же при синхронной фиксации отменяется необходимость в перехвате исключения *WakeupException*.

  ::
  
   private boolean doCommitSync() {
     try {
       consumer.commitSync();
       return true;
     } catch (CommitFailedException e) {
       // the commit failed with an unrecoverable error. if there is any
       // internal state which depended on the commit, you can clean it
       // up here. otherwise it's reasonable to ignore the error and go on
       log.debug("Commit failed", e);
       return false;
     }
   }
   
   public void run() {
     try {
       consumer.subscribe(topics);
   
       while (true) {
         ConsumerRecords<K, V> records = consumer.poll(Long.MAX_VALUE);
         if (doCommitSync())
           records.forEach(record -> process(record));
       }
     } catch (WakeupException e) {
       // ignore, we're closing
     } catch (Exception e) {
       log.error("Unexpected error", e);
     } finally {
       consumer.close();
       shutdownLatch.countDown();
     }
   }


**C/C++** (*librdkafka*):

  ::
  
   void consume_loop(rd_kafka_t *rk,
                     rd_kafka_topic_partition_list_t *topics) {
     rd_kafka_resp_err_t err;
   
     if ((err = rd_kafka_subscribe(rk, topics))) {
       fprintf(stderr, "%% Failed to start consuming topics: %s\n", rd_kafka_err2str(err));
       exit(1);
     }
   
     while (running) {
       rd_kafka_message_t *rkmessage = rd_kafka_consumer_poll(rk, 500);
       if (rkmessage && !rd_kafka_commit_message(rk, rkmessage, 0)) {
         msg_process(rkmessage);
         rd_kafka_message_destroy(rkmessage);
       }
     }
   
     err = rd_kafka_consumer_close(rk);
     if (err)
       fprintf(stderr, "%% Failed to close consumer: %s\n", rd_kafka_err2str(err));
     else
       fprintf(stderr, "%% Consumer closed\n");
   }

**Python**:

  ::
  
   def consume_loop(consumer, topics):
       try:
           consumer.subscribe(topics)
   
           while running:
               msg = consumer.poll(timeout=1.0)
               if msg is None: continue
   
               if msg.error():
                   if msg.error().code() == KafkaError._PARTITION_EOF:
                       # End of partition event
                       sys.stderr.write('%% %s [%d] reached end at offset %d\n' %
                                        (msg.topic(), msg.partition(), msg.offset()))
                   elif msg.error():
                       raise KafkaException(msg.error())
               else:
                   consumer.commit(async=False)
                   msg_process(msg)
   
       finally:
           # Close down consumer to commit final offsets.
           consumer.close()

**Go**:

  ::
  
   for run == true {
       ev := consumer.Poll(0)
       switch e := ev.(type) {
       case *kafka.Message:
           err = consumer.CommitMessage(e)
           if err == nil {
               msg_process(e)
           }
   
       case kafka.PartitionEOF:
           fmt.Printf("%% Reached %v\n", e)
       case kafka.Error:
           fmt.Fprintf(os.Stderr, "%% Error: %v\n", e)
           run = false
       default:
           fmt.Printf("Ignored %v\n", e)
       }
   }

**C#**:

  ::
  
   consumer.OnMessage += (_, msg) =>
   {
       var err = consumer.CommitAsync().Result.Error;
       if (!err)
       {
           processMessage(msg);
       }
   }
   
   consumer.OnPartitionEOF += (_, end)
       => Console.WriteLine($"Reached end of topic {end.Topic} partition {end.Partition}.");
   
   consumer.OnError += (_, error)
   {
       Console.WriteLine($"Error: {error}");
       cancelled = true;
   }
   
   while (!cancelled)
   {
       consumer.Poll(TimeSpan.FromSeconds(1));
   }


Для простоты в примере *rd_kafka_commit_message* используется перед обработкой сообщения, так как фиксация каждого сообщения на практике приводит к большим накладным расходам. Поэтому лучшим подходом является сбор пакета сообщений, выполнение синхронного коммита и затем после успешной фиксации обработка сообщений.

.. important:: Правильное управление смещением имеет решающее значение, поскольку оно влияет на семантику доставки


Асинхронные коммиты
^^^^^^^^^^^^^^^^^^^^^

Каждый вызов commit API приводит к отправке брокеру запроса на фиксацию смещения. При использовании синхронного API потребитель блокируется до тех пор, пока запрос не будет успешно возвращен. Это может снизить общую пропускную способность, поскольку в противном случае потребитель мог бы обрабатывать записи, ожидающие фиксации. Одним из способов решения этой проблемы является увеличение объема данных, возвращаемых в каждом *poll()*, через параметр конфигурации *fetch.min.bytes*. Тогда брокер удерживает выборку до тех пор, пока не будет достигнуто достаточное количество данных (или не истечет срок *fetch.max.wait.ms*). Побочный эффект заключается в том, что способ также увеличивает количество дубликатов, с которыми приходится сталкиваться при случае сбоя.

Второй вариант -- использовать асинхронные коммиты. Потребитель может отправить запрос и, не дожидаясь завершения запроса, немедленно вернуться.

  ::
  
   public void run() {
     try {
       consumer.subscribe(topics);
   
       while (true) {
         ConsumerRecords<K, V> records = consumer.poll(Long.MAX_VALUE);
         records.forEach(record -> process(record));
         consumer.commitAsync();
       }
     } catch (WakeupException e) {
       // ignore, we're closing
     } catch (Exception e) {
       log.error("Unexpected error", e);
     } finally {
       consumer.close();
       shutdownLatch.countDown();
     }
   }


**C/C++** (*librdkafka*):

  ::
  
   void consume_loop(rd_kafka_t *rk,
                     rd_kafka_topic_partition_list_t *topics) {
     static const int MIN_COMMIT_COUNT = 1000;
   
     int msg_count = 0;
     rd_kafka_resp_err_t err;
   
     if ((err = rd_kafka_subscribe(rk, topics))) {
       fprintf(stderr, "%% Failed to start consuming topics: %s\n", rd_kafka_err2str(err));
       exit(1);
     }
   
     while (running) {
       rd_kafka_message_t *rkmessage = rd_kafka_consumer_poll(rk, 500);
       if (rkmessage) {
         msg_process(rkmessage);
         rd_kafka_message_destroy(rkmessage);
   
         if ((++msg_count % MIN_COMMIT_COUNT) == 0)
           rd_kafka_commit(rk, NULL, 1);
       }
     }
   
     err = rd_kafka_consumer_close(rk);
     if (err)
       fprintf(stderr, "%% Failed to close consumer: %s\n", rd_kafka_err2str(err));
     else
       fprintf(stderr, "%% Consumer closed\n");
   }


Единственное различие между этим примером и предыдущим заключается в том, что в вызове *rd_kafka_commit* включена асинхронная фиксация.

Изменения в **Python** очень похожи. Параметр *async* для *commit()* изменен на *True*. В примере значение передается явно, но асинхронная фиксация используется по умолчанию, если параметр не включен:

  ::
  
   def consume_loop(consumer, topics):
       try:
           consumer.subscribe(topics)
   
           msg_count = 0
           while running:
               msg = consumer.poll(timeout=1.0)
               if msg is None: continue
   
               if msg.error():
                   if msg.error().code() == KafkaError._PARTITION_EOF:
                       # End of partition event
                       sys.stderr.write('%% %s [%d] reached end at offset %d\n' %
                                        (msg.topic(), msg.partition(), msg.offset()))
                   elif msg.error():
                       raise KafkaException(msg.error())
               else:
                   msg_process(msg)
                   msg_count += 1
                   if msg_count % MIN_COMMIT_COUNT == 0:
                       consumer.commit(async=True)
       finally:
           # Close down consumer to commit final offsets.
           consumer.close()

В **Go**  для асинхронной фиксации необходимо выполнить коммит в goroutine:

  ::
  
   msg_count := 0
   for run == true {
       ev := consumer.Poll(0)
       switch e := ev.(type) {
       case *kafka.Message:
           msg_count += 1
           if msg_count % MIN_COMMIT_COUNT == 0 {
               go func() {
                   offsets, err := consumer.Commit()
               }()
           }
           fmt.Printf("%% Message on %s:\n%s\n",
               e.TopicPartition, string(e.Value))
   
       case kafka.PartitionEOF:
           fmt.Printf("%% Reached %v\n", e)
       case kafka.Error:
           fmt.Fprintf(os.Stderr, "%% Error: %v\n", e)
           run = false
       default:
           fmt.Printf("Ignored %v\n", e)
       }
   }


В **C#**  для асинхронной фиксации необходимо вызвать метод *CommitAsync*:

  ::
  
   var msgCount = 0;
   
   consumer.OnMessage += (_, msg) =>
   {
       processMessage(msg);
       msgCount += 1;
       if (msgCount % MIN_COMMIT_COUNT == 0)
       {
           consumer.CommitAsync();
       }
   }
   
   consumer.OnPartitionEOF += (_, end)
       => Console.WriteLine($"Reached end of topic {end.Topic} partition {end.Partition}.");
   
   consumer.OnError += (_, error)
   {
       Console.WriteLine($"Error: {error}");
       cancelled = true;
   }
   
   while (!cancelled)
   {
       consumer.Poll(TimeSpan.FromSeconds(1));
   }


Поскольку такой способ помогает производительности, почему бы всегда не использовать асинхронные коммиты? Основная причина заключается в том, что потребитель не повторяет запрос в случае сбоя фиксации. Это то, что *commitSync* предлагает даром; он повторяется бесконечно, пока фиксация не будет выполнена, или не будет найдена неисправимая ошибка. Проблема с асинхронными коммитами связана с порядком фиксации -- к тому времени, когда потребитель узнает, что фиксация не удалась, возможно, уже будет обработан следующий пакет сообщений, и даже будет отправлен следующий коммит. В этом случае повторная попытка старой фиксации может привести к дублированию.

Вместо того, чтобы усложнять свойства потребителей в попытках самостоятельного решения этой проблемы, API выдает обратный запрос при свершении коммита -- и успешного, и при неудаче. При желании можно использовать этот обратный запрос для повторной фиксации, но тогда приходится сталкиваться с проблемой переназначения.

  ::
  
   public void run() {
     try {
       consumer.subscribe(topics);
   
       while (true) {
         ConsumerRecords<K, V> records = consumer.poll(Long.MAX_VALUE);
         records.forEach(record -> process(record));
         consumer.commitAsync(new OffsetCommitCallback() {
           public void onComplete(Map<TopicPartition, OffsetAndMetadata> offsets, Exception exception) {
             if (e != null)
               log.debug("Commit failed for offsets {}", offsets, e);
             }
         });
       }
     } catch (WakeupException e) {
       // ignore, we're closing
     } catch (Exception e) {
       log.error("Unexpected error", e);
     } finally {
       consumer.close();
       shutdownLatch.countDown();
     }
   }

Аналогичная функция доступна в **C/C++** (*librdkafka*), но ее необходимо настроить при инициализации:

  ::
  
   static void on_commit(rd_kafka_t *rk,
                         rd_kafka_resp_err_t err,
                         rd_kafka_topic_partition_list_t *offsets,
                         void *opaque) {
     if (err)
       fprintf(stderr, "%% Failed to commit offsets: %s\n", rd_kafka_err2str(err));
   }
   
   void init_rd_kafka() {
     rd_kafka_conf_t *conf = rd_kafka_conf_new();
     rd_kafka_conf_set_offset_commit_cb(conf, on_commit);
   
     // initialization omitted
   }

Аналогично, в **Python** обратный запрос может быть вызван любым коммитом и может быть передан в качестве параметра конфигурации конструктора потребителя:

  ::
  
   from confluent_kafka import Consumer
   
   def commit_completed(err, partitions):
       if err:
           print(str(err))
       else:
           print("Committed partition offsets: " + str(partitions))
   
   conf = {'bootstrap.servers': "host1:9092,host2:9092",
           'group.id': "foo",
           'default.topic.config': {'auto.offset.reset': 'smallest'},
           'on_commit': commit_completed}
   
   consumer = Consumer(conf)


В **C#** можно использовать *Task*:

  ::
  
   var msgCount = 0;
   
   consumer.OnMessage += (_, msg) =>
   {
       processMessage(msg);
       msgCount += 1;
       if (msgCount % MIN_COMMIT_COUNT == 0)
       {
           consumer.CommitAsync().ContinueWith(
               commitResult =>
               {
                   if (commitResult.Error)
                   {
                       Console.Error.WriteLine(commitResult.Error);
                   }
                   else
                   {
                       Console.WriteLine(
                           $"Committed Offsets [{string.Join(", ", commitResult.Offsets)}]");
                   }
               }
           )
       }
   }


В **Go** события перебалансировки отображаются как события, возвращаемые методом *Poll()*. Для того чтобы увидеть эти события, необходимо создать потребителя с конфигурацией *go.application.rebalance.enable* и обработать события *AssignedPartitions* и *RevokedPartitions*, явно вызвав *Assign()* и *Unassign()* для *AssignedPartitions* и *RevokedPartitions* соответственно:

  ::
  
   consumer, err := kafka.NewConsumer(&kafka.ConfigMap{
        "bootstrap.servers":    "host1:9092,host2:9092",
        "group.id":             "foo",
        "go.application.rebalance.enable": true})
   
   msg_count := 0
   for run == true {
       ev := consumer.Poll(0)
       switch e := ev.(type) {
       case kafka.AssignedPartitions:
           fmt.Fprintf(os.Stderr, "%% %v\n", e)
           c.Assign(e.Partitions)
       case kafka.RevokedPartitions:
           fmt.Fprintf(os.Stderr, "%% %v\n", e)
           c.Unassign()
       case *kafka.Message:
           msg_count += 1
           if msg_count % MIN_COMMIT_COUNT == 0 {
               consumer.Commit()
           }
   
           fmt.Printf("%% Message on %s:\n%s\n",
               e.TopicPartition, string(e.Value))
   
       case kafka.PartitionEOF:
           fmt.Printf("%% Reached %v\n", e)
       case kafka.Error:
           fmt.Fprintf(os.Stderr, "%% Error: %v\n", e)
           run = false
       default:
           fmt.Printf("Ignored %v\n", e)
       }
   }


Сбои фиксации смещения досаждают, когда последующие коммиты успешны, так как фактически они не должны приводить к повторным чтениям. Однако, если последняя фиксация завершается неудачно до того, как происходит перебалансировка или отключение потребителя, смещения сбрасываются до последнего коммита, и вероятнее всего, отображаются дубликаты. Поэтому общая схема заключается в том, чтобы объединить асинхронные коммиты в цикле опроса с синхронизированными коммитами при перебалансировках или отключении. Фиксация при отключении несложна, но необходимо найти способ закрепления поведения при перебалансировке. Для этого представленный ранее метод *subscribe()* имеет вариант, принимающий *ConsumerRebalanceListener*, который имеет два метода закрепления поведения перебалансировки.

В следующем примере синхронные фиксации включаются при перебалансировках и при отключении:

  ::
  
   private void doCommitSync() {
     try {
       consumer.commitSync();
     } catch (WakeupException e) {
       // we're shutting down, but finish the commit first and then
       // rethrow the exception so that the main loop can exit
       doCommitSync();
       throw e;
     } catch (CommitFailedException e) {
       // the commit failed with an unrecoverable error. if there is any
       // internal state which depended on the commit, you can clean it
       // up here. otherwise it's reasonable to ignore the error and go on
       log.debug("Commit failed", e);
     }
   }
   
   public void run() {
     try {
       consumer.subscribe(topics, new ConsumerRebalanceListener() {
         @Override
         public void onPartitionsRevoked(Collection<TopicPartition> partitions) {
           doCommitSync();
         }
   
         @Override
         public void onPartitionsAssigned(Collection<TopicPartition> partitions) {}
       });
   
       while (true) {
         ConsumerRecords<K, V> records = consumer.poll(Long.MAX_VALUE);
         records.forEach(record -> process(record));
         consumer.commitAsync();
       }
     } catch (WakeupException e) {
       // ignore, we're closing
     } catch (Exception e) {
       log.error("Unexpected error", e);
     } finally {
       try {
         doCommitSync();
       } finally {
         consumer.close();
         shutdownLatch.countDown();
       }
     }
   }


Каждая перебалансировка имеет две фазы: отзыв и назначение партиции. Отзыв партиции всегда вызывается перед перебалансировкой и является последним шансом фиксации смещения перед переназначением. Фаза назначения партиции всегда вызывается после перебалансировки и может использоваться для установки начальной позиции назначенных партиций. В этом случае отзыв используется для синхронной фиксации текущих смещений.

Как правило, асинхронные коммиты следует считать менее безопасными, чем синхронные, так как последовательные неудачи фиксации приводят к увеличению обработки дубликатов. Можно снизить эту опасность, добавив логику для обработки ошибок фиксации в обратном запросе или периодически смешивая с вызовами *commitSync()*, но не следует добавлять слишком много сложностей при отсутствии прямой необходимости. При синхронных коммитах можно повысить надежность, увеличив число партиций топика и количество потребителей в группе. А если необходимо максимизировать пропускную способность при готовности некоторого увеличения числа дубликатов, то асинхронные коммиты могут стать хорошим вариантом.

Довольно очевидный момент, но стоит отметить, что асинхронные коммиты имеют смысл только для "at least once" доставки сообщений. Чтобы получить "at most once", прежде чем считывать сообщение необходимо знать, успешна ли фиксация. А это подразумевает синхронную фиксацию, за исключением случая наличия возможности "непрочтения" сообщения после того, как обнаружится, что фиксация не удалась.


Мониторинг групп
^^^^^^^^^^^^^^^^^^^^^

**Kafka** включает в себя утилиту администратора для просмотра статуса групп потребителей.

Для получения списка активных групп в кластере можно использовать утилиту **kafka-consumer-groups**, включенную в дистрибутив **Kafka**. В большом кластере это может занять некоторое время, поскольку она собирает список путем проверки каждого брокера.

  ::
  
   bin/kafka-consumer-groups --bootstrap-server host:9092 --list


Утилита **kafka-consumer-groups** также может быть использована для сбора информации о текущей группе. Пример просмотра актуальных назначений для группы *foo*:

  ::
  
   bin/kafka-consumer-groups --bootstrap-server host:9092 --describe --group foo


В случае вызова утилиты во время перебалансировки, команда сообщает об ошибке. Тогда необходимо повторить попытку, в результате которой отображаются назначения для всех членов в текущей группе.
