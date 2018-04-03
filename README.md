- [Документация по Arenadata Streaming](#Документация-по-arenadata-streaming)
  - [Основные понятия](#Основные-понятия)
    - [Введение в Arenadata Streaming](#Введение-в-arenadata-streaming)
    - [Концепция хранения](#Концепция-хранения)
    - [Guarantees](#Guarantees)
    - [Рекомендации по использованию](#Рекомендации-по-использованию)
  - [Архитектурные особенности](#Архитектурные-особенности)
    - [Persistence](#Persistence)
    - [Эффективность](#Эффективность)
    - [Producers](#Producers)
    - [Consumers](#Consumers)
    - [Механизм доставки сообщений](#Механизм-доставки-сообщений)
    - [Репликация](#Репликация) 
    - [Сжатие журналов](#Сжатие-журналов
  - [APIs](#APIs)
    - [Producer API](#Producer-API)
    - [Concumer API](#Concumer-API)
    - [Streams API](#Streams-API)
    - [Connect API](#Connect-API)
    - [AdminClient API](#AdminClien-API)
    - [Legasy API](#Legasy-API)
  - [Конфигурирование](#Конфигурирование)
    - [Настройки брокера](#Настройки-брокера)
    - [Настройки на уровне топика](#Настройки-на-уровне-топика)
    - [Конфигурирование Producer](#Конфигурирование-Producer)
    - [Конфигурирование Consumer](#Конфигурирование-Consumer)
    - [Конфигурирование Connect](#Конфигурирование-Connect)
    - [Конфигурирование Streams](#Конфигурирование-Streams)
   
# Документация по Arenadata Streams
## Основные понятия
Распределить по главам  - https://kafka.apache.org/intro
### Введение в Arenadata Streaming
### Концепция хранения (Topics and Logs+Distribution+Geo-Replication - все вместе не выделяя каждый из подпунктов отдельным заголовком)
### Guarantees
https://kafka.apache.org/intro#intro_guarantees
### Рекомендации по использованию (Kafka as a Messaging System+Kafka as a Storage System+Kafka for Stream Processing-каждый из подпунктов выделить отдельным заголовком)
## Архитектурные особенности
Основная часть - https://kafka.apache.org/documentation/#majordesignelements
### Persistence
Основная часть - https://kafka.apache.org/documentation/#persistence
### Эффективность
Основная часть - https://kafka.apache.org/documentation/#maximizingefficiency
### Producers
https://kafka.apache.org/intro#intro_producers
https://kafka.apache.org/documentation/#theproducer
### Consumers
https://kafka.apache.org/intro#intro_consumers
https://kafka.apache.org/documentation/#theconsumer
https://kafka.apache.org/documentation/#impl_offsettracking
Возможно вот это потом нужно будет перенести в конфигурирование https://kafka.apache.org/documentation/#offsetmigration
### Механизм доставки сообщений
https://kafka.apache.org/documentation/#semantics
### Репликация
Основная часть - https://kafka.apache.org/documentation/#replication
### Сжатие журналов
Основная часть(Log Compaction(выборочно)+Log Compaction Basics+What guarantees does log compaction provide?) -
https://kafka.apache.org/documentation/#compaction
https://kafka.apache.org/documentation/#design_compactionbasics 
https://kafka.apache.org/documentation/#design_compactionguarantees
Возможно вот это потом нужно будет перенести в конфигурирование https://kafka.apache.org/documentation/#design_compactiondetails
## APIs
Вступление - https://kafka.apache.org/documentation/#api
### Producer API
Основная часть - https://kafka.apache.org/documentation/#producerapi
### Concumer API
Основная часть - https://kafka.apache.org/documentation/#consumerapi
### Streams API
Основная часть - https://kafka.apache.org/documentation/#streamsapi
### Connect API
Основная часть - https://kafka.apache.org/documentation/#connectapi
### AdminClient API
Основная часть - https://kafka.apache.org/documentation/#adminapi
### Legasy API
Основная часть - https://kafka.apache.org/documentation/#legacyapis
## Конфигурирование 
Вступление - https://kafka.apache.org/documentation/#configuration
### Настройки брокера
Таблица - https://kafka.apache.org/documentation/#brokerconfigs
### Настройки на уровне топика
Таблица - https://kafka.apache.org/documentation/#topicconfigs
### Конфигурирование Producer
Таблица - https://kafka.apache.org/documentation/#producerconfigs
### Конфигурирование Consumer
https://kafka.apache.org/documentation/#newconsumerconfigs
### Конфигурирование Connect
https://kafka.apache.org/documentation/#connectconfigs
### Конфигурирование Streams
https://kafka.apache.org/documentation/#streamsconfigs

## Operations (TODO https://kafka.apache.org/documentation/#operations)

