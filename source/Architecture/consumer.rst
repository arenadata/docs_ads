The Consumer
==============

The **ADS** consumer works by issuing "fetch" requests to the brokers leading the partitions it wants to consume. The consumer specifies its offset in the log with each request and receives back a chunk of log beginning from that position. The consumer thus has significant control over this position and can rewind it to re-consume data if need be.

An initial question we considered is whether consumers should pull data from brokers or brokers should push data to the consumer. In this respect **ADS** follows a more traditional design, shared by most messaging systems, where data is pushed to the broker from the producer and pulled from the broker by the consumer. Some logging-centric systems, such as **Scribe** and **Apache Flume**, follow a very different push-based path where data is pushed downstream. There are pros and cons to both approaches. However, a push-based system has difficulty dealing with diverse consumers as the broker controls the rate at which data is transferred. The goal is generally for the consumer to be able to consume at the maximum possible rate; unfortunately, in a push system this means the consumer tends to be overwhelmed when its rate of consumption falls below the rate of production (a denial of service attack, in essence). A pull-based system has the nicer property that the consumer simply falls behind and catches up when it can. This can be mitigated with some kind of backoff protocol by which the consumer can indicate it is overwhelmed, but getting the rate of transfer to fully utilize (but never over-utilize) the consumer is trickier than it seems. Previous attempts at building systems in this fashion led us to go with a more traditional pull model.

Another advantage of a pull-based system is that it lends itself to aggressive batching of data sent to the consumer. A push-based system must choose to either send a request immediately or accumulate more data and then send it later without knowledge of whether the downstream consumer will be able to immediately process it. If tuned for low latency, this will result in sending a single message at a time only for the transfer to end up being buffered anyway, which is wasteful. A pull-based design fixes this as the consumer always pulls all available messages after its current position in the log (or up to some configurable max size). So one gets optimal batching without introducing unnecessary latency.

The deficiency of a naive pull-based system is that if the broker has no data the consumer may end up polling in a tight loop, effectively busy-waiting for data to arrive. To avoid this **ADS** has parameters in pull request that allow the consumer request to block in a "long poll" waiting until data arrives (and optionally waiting until a given number of bytes is available to ensure large transfer sizes).


Consumer Position
-------------------

Keeping track of what has been consumed is, surprisingly, one of the key performance points of a messaging system.

Most messaging systems keep metadata about what messages have been consumed on the broker. That is, as a message is handed out to a consumer, the broker either records that fact locally immediately or it may wait for acknowledgement from the consumer. This is a fairly intuitive choice, and indeed for a single machine server it is not clear where else this state could go. Since the data structures used for storage in many messaging systems scale poorly, this is also a pragmatic choice -- since the broker knows what is consumed it can immediately delete it, keeping the data size small.

What is perhaps not obvious is that getting the broker and consumer to come into agreement about what has been consumed is not a trivial problem. If the broker records a message as consumed immediately every time it is handed out over the network, then if the consumer fails to process the message (say because it crashes or the request times out or whatever) that message will be lost. To solve this problem, many messaging systems add an acknowledgement feature which means that messages are only marked as sent not consumed when they are sent; the broker waits for a specific acknowledgement from the consumer to record the message as consumed. This strategy fixes the problem of losing messages, but creates new problems. First of all, if the consumer processes the message but fails before it can send an acknowledgement then the message will be consumed twice. The second problem is around performance, now the broker must keep multiple states about every single message (first to lock it so it is not given out a second time, and then to mark it as permanently consumed so that it can be removed). Tricky problems must be dealt with, like what to do with messages that are sent but never acknowledged.

**ADS** handles this differently. Our topic is divided into a set of totally ordered partitions, each of which is consumed by exactly one consumer within each subscribing consumer group at any given time. This means that the position of a consumer in each partition is just a single integer, the offset of the next message to consume. This makes the state about what has been consumed very small, just one number for each partition. This state can be periodically checkpointed. This makes the equivalent of message acknowledgements very cheap.

There is a side benefit of this decision. A consumer can deliberately rewind back to an old offset and re-consume data. This violates the common contract of a queue, but turns out to be an essential feature for many consumers. For example, if the consumer code has a bug and is discovered after some messages are consumed, the consumer can re-consume those messages once the bug is fixed.


Offline Data Load
-------------------

Scalable persistence allows for the possibility of consumers that only periodically consume such as batch data loads that periodically bulk-load data into an offline system such as **Hadoop** or a relational data warehouse.

In the case of **Hadoop** we parallelize the data load by splitting the load over individual map tasks, one for each node/topic/partition combination, allowing full parallelism in the loading. **Hadoop** provides the task management, and tasks which fail can restart without danger of duplicate data -- they simply restart from their original position.


Consumer Offset Tracking
--------------------------

The consumer tracks the maximum offset it has consumed in each partition and has the capability to commit offsets so that it can resume from those offsets in the event of a restart. **ADS** provides the option to store all the offsets for a given consumer group in a designated broker (for that group) called the group coordinator. i.e., any consumer instance in that consumer group should send its offset commits and fetches to that group coordinator (broker). Consumer groups are assigned to coordinators based on their group names. A consumer can look up its coordinator by issuing a *FindCoordinatorRequest* to any **ADS** broker and reading the *FindCoordinatorResponse* which will contain the coordinator details. The consumer can then proceed to commit or fetch offsets from the coordinator broker. In case the coordinator moves, the consumer will need to rediscover the coordinator. Offset commits can be done automatically or manually by consumer instance.

Потребитель высокого уровня отслеживает свое максимальное смещение при считывании данных в каждой партиции и периодически фиксирует вектор смещения для возможности возобновления работы с необходимых позиций. **ADS** предоставляет возможность хранения всех смещений группы потребителей в определенном брокере (для каждой группы) -- в менеджере смещений, то есть любой инстанс потребителя группы должен передавать свои смещения и запрашивать выборки через менеджер смещения (брокер). У потребителей высокого уровня эта операция выполняется автоматически, в то время как простой потребитель управляет своими смещениями вручную, так как в настоящее время автоматическая передача смещений простых потребителей не поддерживается **Java**, им доступно только ручное фиксирование или извлечение смещений в **ZooKeeper**. При использовании **Scala** простой потребитель может явно зафиксировать или получить смещения в менеджере. 

Поиск менеджера смещения осуществляется по запросу *GroupCoordinatorRequest* в любой брокер **ADS** с последующим ответом *GroupCoordinatorResponse*, в котором содержится менеджер смещения. После чего потребитель может перейти к фиксации или извлечению смещений. В случае перемещения менеджера смещений потребителю необходимо повторно выполнить его поиск.

При получении менеджером смещения запроса на фиксацию *OffsetCommitRequest*, он добавляет смещение в специальный сжатый топик **ADS** с именем *__consumer_offsets*. Менеджер смещения отправляет успешный ответ фиксации смещения потребителю только после получения смещений всеми репликами топика. В случае если смещения не могут реплицироваться в пределах настренного времени ожидания, фиксация смещения завершается неудачей, и потребитель может повторить ее после отмены действия (у потребителей высокого уровня процедура выполняется автоматически). Брокеры периодически сжимают топик смещения, так как ему требуется поддерживать только последнее смещение на партицию. Менеджер смещения также упорядоченно кэширует все смещения в таблице in-memory для возможности их быстрой обработки.

Когда менеджер смещения получает запрос на извлечение смещения, он просто возвращает последний зафиксированный вектор смещения из кэша. В случае если менеджер был только что запущен или стал менеджером смещения для нового набора групп потребителей (став лидером для партиции топика смещения), может потребоваться загрузка партиции топика смещений в кэш. В это время операции по извлечению смещения завершаются ошибкой *OffsetsLoadInProgress*, и потребитель может повторить запрос *OffsetFetchRequest* после окончания копирования (у потребителей высокого уровня процедура выполняется автоматически).


Миграция смещений из ZooKeeper в ADS
--------------------------------------

Для миграции потребителей и смещений из **ZooKeeper** в **ADS** необходимо выполнить следующие действия:

1. Установить *offsets.storage=ads* и *dual.commit.enabled=true* в настройках потребителя.
2. Выполнить резкий переход к потребителям и убедиться в их исправности.
3. Установить *dual.commit.enabled=false* в настройках потребителя.
4. Выполнить резкий переход к потребителям и убедиться в их исправности.

Обратный переход (с **ADS** в **ZooKeeper**) также может выполняться с помощью вышеуказанных шагов при установке *offsets.storage=zookeeper*.
