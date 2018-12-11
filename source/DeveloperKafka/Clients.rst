Kafka Clients
==============

Kлиенты для работы с Apache Kafka доступны на многих языках программирования (`Clients <https://cwiki.apache.org/confluence/display/KAFKA/Clients>`_).

В **ADS** предусмотрена возможность установки клиентских библиотек для различных языков, которые обеспечивают как низкоуровневый доступ к **Kafka**, так и потоковую обработку более высокого уровня.


.. csv-table:: Клиентские библиотки для различных языков
   :header: "Ссылка на установку", "Ссылка на документацию"
   :widths: 20, 80

   "`C/C++ <>`_", "`github.com/edenhill/librdkafka <https://github.com/edenhill/librdkafka>`_"
   "`Go <>`_", "`github.com/confluentinc/confluent-kafka-go <https://github.com/confluentinc/confluent-kafka-go/>`_"
   "`Java <>`_", "`Kafka Java Consumer <https://docs.confluent.io/current/clients/consumer.html#kafka-consumer>`_ и `Kafka Java Producer <https://docs.confluent.io/current/clients/producer.html#kafka-producer>`_"
   "`JMS <>`_", "`JMS Client <https://docs.confluent.io/current/clients/kafka-jms-client/docs/index.html#client-jms>`_"
   "`.NET <>`_", "`github.com/confluentinc/confluent-kafka-dotnet <https://github.com/confluentinc/confluent-kafka-dotnet>`_"
   "`Python <>`_", "`github.com/confluentinc/confluent-kafka-python <https://github.com/confluentinc/confluent-kafka-python>`_"  

 
В следующей таблице описана поддержка клиента по различным функциям платформы **ADS**.

.. csv-table:: Поддержка клиента по различным функциям платформы
   :header: "Функция", "C/C++", "Go", "Java", ".NET", "Python"
   :widths: 50, 10, 10, 10, 10, 10

   "Admin API", "Да", "Да", "Да", "Нет", "Да"
   "Control Center metrics integration", "Да", "Да", "Да", "Да", "Да"
   "Custom partitioner", "Да", "Нет", "Да", "Нет", "Нет"
   "Exactly Once Semantics", "Нет", "Нет", "Да", "Нет", "Нет"
   "Idempotent Producer", "Нет", "Нет", "Да", "Нет", "Нет"
   "Kafka Streams", "Нет", "Нет", "Да", "Нет", "Нет"
   "Record Headers", "Да", "Да", "Да", "Да", "Да"
   "SASL Kerberos/GSSAPI", "Да", "Да", "Да", "Да", "Да"
   "SASL PLAIN", "Да", "Да", "Да", "Да", "Да"
   "SASL SCRAM", "Да", "Да", "Да", "Да", "Да"
   "Simplified installation", "Да", "Нет", "Да", "Да", "Да"
   "Schema Registry", "Да", "Нет", "Да", "Да", "Да"
   "Topic Metadata API", "Да", "Да", "Да", "Нет", "Да" 


Java. Установка
-----------------




C/C++. Установка
-----------------




JMS. Установка
-----------------




Python. Установка
------------------




Go. Установка
-----------------





