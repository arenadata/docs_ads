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

**index.interval.bytes** -- This setting controls how frequently Kafka adds an index entry to it's offset index. The default setting ensures that we index a message roughly every *4096* bytes. More indexing allows reads to jump closer to the exact position in the log but makes the index larger. You probably don't need to change this

+ TYPE -- int
+ DEFAULT -- 4096
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.index.interval.bytes
+ IMPORTANCE -- medium

**leader.replication.throttled.replicas** -- A list of replicas for which log replication should be throttled on the leader side. The list should describe a set of replicas in the form "[PartitionId]:[BrokerId],[PartitionId]:[BrokerId]:..." or alternatively the wildcard "*" can be used to throttle all replicas for this topic

+ TYPE -- list
+ DEFAULT -- ""
+ VALID VALUES -- [partitionId],[brokerId]:[partitionId],[brokerId]:...
+ SERVER DEFAULT PROPERTY -- leader.replication.throttled.replicas
+ IMPORTANCE -- medium

**max.message.bytes** -- The largest record batch size allowed by **ADS**. If this is increased the consumers' fetch size must also be increased so that the they can fetch record batches this large

+ TYPE -- int
+ DEFAULT -- 1000012
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- message.max.bytes
+ IMPORTANCE -- medium

**message.format.version** -- Specify the message format version the broker will use to append messages to the logs. The value should be a valid ApiVersion. Some examples are: "0.8.2", "0.9.0.0", "0.10.0", check ApiVersion for more details. By setting a particular message format version, the user is certifying that all the existing messages on disk are smaller or equal than the specified version. Setting this value incorrectly will cause consumers with older versions to break as they will receive messages with a format that they don't understand

+ TYPE -- string
+ DEFAULT -- 1.1-IV0
+ SERVER DEFAULT PROPERTY -- log.message.format.version
+ IMPORTANCE -- medium

**message.timestamp.difference.max.ms** -- The maximum difference allowed between the timestamp when a broker receives a message and the timestamp specified in the message. If *message.timestamp.type=CreateTime*, a message will be rejected if the difference in timestamp exceeds this threshold. This configuration is ignored if *message.timestamp.type=LogAppendTime*

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.message.timestamp.difference.max.ms
+ IMPORTANCE -- medium

**message.timestamp.type** -- Define whether the timestamp in the message is message create time or log append time. The value should be either "CreateTime" or "LogAppendTime"

+ TYPE -- string
+ DEFAULT -- CreateTime
+ SERVER DEFAULT PROPERTY -- log.message.timestamp.type
+ IMPORTANCE -- medium

**min.cleanable.dirty.ratio** -- This configuration controls how frequently the log compactor will attempt to clean the log (assuming log compaction is enabled). By default we will avoid cleaning a log where more than *50%* of the log has been compacted. This ratio bounds the maximum space wasted in the log by duplicates (at *50%* at most *50%* of the log could be duplicates). A higher ratio will mean fewer, more efficient cleanings but will mean more wasted space in the log

+ TYPE -- double
+ DEFAULT -- 0.5
+ VALID VALUES -- [0,...,1]
+ SERVER DEFAULT PROPERTY -- log.cleaner.min.cleanable.ratio
+ IMPORTANCE -- medium

**min.compaction.lag.ms** -- The minimum time a message will remain uncompacted in the log. Only applicable for logs that are being compacted

+ TYPE -- long
+ DEFAULT -- 0
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.cleaner.min.compaction.lag.ms
+ IMPORTANCE -- medium

**min.insync.replicas** -- When a producer sets acks to "all" (or "-1"), this configuration specifies the minimum number of replicas that must acknowledge a write for the write to be considered successful. If this minimum cannot be met, then the producer will raise an exception (either *NotEnoughReplicas* or *NotEnoughReplicasAfterAppend*). When used together, *min.insync.replicas* and acks allow you to enforce greater durability guarantees. A typical scenario would be to create a topic with a replication factor of "3", set *min.insync.replicas* to "2", and produce with acks of "all". This will ensure that the producer raises an exception if a majority of replicas do not receive a write

+ TYPE -- int
+ DEFAULT -- 1
+ VALID VALUES -- [1,...]
+ SERVER DEFAULT PROPERTY -- min.insync.replicas
+ IMPORTANCE -- medium

**preallocate** -- True if we should preallocate the file on disk when creating a new log segment

+ TYPE -- boolean
+ DEFAULT -- false
+ SERVER DEFAULT PROPERTY -- log.preallocate
+ IMPORTANCE -- medium

**retention.bytes** -- This configuration controls the maximum size a partition (which consists of log segments) can grow to before we will discard old log segments to free up space if we are using the "delete" retention policy. By default there is no size limit only a time limit. Since this limit is enforced at the partition level, multiply it by the number of partitions to compute the topic retention in bytes

+ TYPE -- long
+ DEFAULT -- - 1
+ SERVER DEFAULT PROPERTY -- log.retention.bytes
+ IMPORTANCE -- medium

**retention.ms** -- This configuration controls the maximum time we will retain a log before we will discard old log segments to free up space if we are using the "delete" retention policy. This represents an SLA on how soon consumers must read their data

+ TYPE -- long
+ DEFAULT -- 604800000
+ SERVER DEFAULT PROPERTY -- log.retention.ms
+ IMPORTANCE -- medium

**segment.bytes** -- This configuration controls the segment file size for the log. Retention and cleaning is always done a file at a time so a larger segment size means fewer files but less granular control over retention

+ TYPE -- int
+ DEFAULT -- 1073741824
+ VALID VALUES -- [14,...]
+ SERVER DEFAULT PROPERTY -- log.segment.bytes
+ IMPORTANCE -- medium

**segment.index.bytes** -- This configuration controls the size of the index that maps offsets to file positions. We preallocate this index file and shrink it only after log rolls. You generally should not need to change this setting

+ TYPE -- int
+ DEFAULT -- 10485760
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.index.size.max.bytes
+ IMPORTANCE -- medium

**segment.jitter.ms** -- The maximum random jitter subtracted from the scheduled segment roll time to avoid thundering herds of segment rolling

+ TYPE -- long
+ DEFAULT -- 0
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.roll.jitter.ms
+ IMPORTANCE -- medium

**segment.ms** -- This configuration controls the period of time after which **ADS** will force the log to roll even if the segment file isn't full to ensure that retention can delete or compact old data

+ TYPE -- long
+ DEFAULT -- 604800000
+ VALID VALUES -- [0,...]
+ SERVER DEFAULT PROPERTY -- log.roll.ms
+ IMPORTANCE -- medium

**unclean.leader.election.enable** -- Indicates whether to enable replicas not in the ISR set to be elected as leader as a last resort, even though doing so may result in data loss

+ TYPE -- boolean
+ DEFAULT -- false
+ SERVER DEFAULT PROPERTY -- unclean.leader.election.enable
+ IMPORTANCE -- medium

