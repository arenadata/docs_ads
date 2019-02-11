
.. _ADS_index_logo:

.. figure:: imgs/ADS_index_logo.*
   :align: center


Distributed streaming platform 
===============================

**Arenadata Streaming** (**ADS**) -- enterprise-ready Streaming Platform that makes it easy build real-time data pipelines and streaming applications based on Apache Kafka and Apache Nifi -- open source projects. **ADS** contains all the necessary components for collecting, analyzing and processing data in real time, provides storage and transmission in the semantics of "exactly-once delivery" in a safe and fault-tolerant way, providing a convenient interface for administration and development. The platform can also act as a corporate data bus and ETL tool (:numref:`Рис.%s.<ADS_index_scheme>`).


.. _ADS_index_scheme:

.. figure:: imgs/ADS_index_scheme.png
   :align: center

   ADS Platform Concept


The idea of a distributed streaming platform is to provide:

+ Single access point;
+ Easy, safe and reliable way to control data flow;
+ Security policies;
+ Fast and continuous development.


One of the features of the implementation of the platform is the use of technology, similar to the transaction logs used in database management systems. **ADS** has the following distinctive technical qualities:

+ *Fault tolerance* -- ensuring consistency in real-time streaming;
+ *Scalability* -- adding new servers to the cluster as needed;
+ *Distribution* -- possibility of building geo-distributed infrastructure;
+ *Available equipment* -- work on any x86-compatible hardware;
+ *Real time* -- stream management, adding and configuring real-time data sources;
+ *Security* -- flexible data access control mechanisms;
+ *Integration* -- connectors to different systems: Elasticsearch, SAP HANA, Vertica, Couchbase, Cassandra, CouchDB, IBM MQ, etc. A wide range of APIs for integration with other external systems;
+ *Simplicity and flexibility* -- the ability to create workflow using a graphical interface or develop your own applications using the SDK to improve performance.

**Apache Kafka** -- distributed software message broker, an open source project developed within the **Apache Software Foundation**. 
 
**Apache NiFi** supports powerful and scalable directed graphs of data routing, transformation, and system mediation logic developed within the **Apache Software Foundation**.

Version **ADS 1.0.0** was released in the second quarter of 2018. The version includes the following components: **Ambari**, **Zookeeper**, **NiFi**, **Kafka**. Integration with **ZooKeeper** allows the system to work not only quickly and smoothly, but safely, which is especially important in the case of big data.

With **Arenadata Streaming** you get an effective solution for stream processing:

+ Use as a corporate data bus for all your applications;
+ Collect large data streams safely and efficiently manage them in real time;
+ Create data streams with support for access rights to them;
+ Develop streaming analytic applications in minutes in real time without writing a line of code.

The manual may be useful for administrators, programmers, developers and employees of information technology departments who implement and maintain clusters **Arenadata**. 

The following documentation describes the platform **Arenadata Streaming**: storage concepts, installation, architectural features and platform settings.

.. important:: Contact information support service -- e-mail: info@arenadata.io

.. important:: Currently, the automatic installation of Arenadata Streaming is only available through Ambari version 2.7.0 or higher


.. toctree::
   :maxdepth: 2
   :caption: Оглавление:

   Intro/index
   Install/index
   Architecture/index
   Config/index
   Release/index
   glossary

.. toctree::
   :hidden:
   :maxdepth: 1
   :caption: Скачать

   download
