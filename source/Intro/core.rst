The core concepts of NiFi
==========================

NiFi’s fundamental design concepts closely relate to the main ideas of `Flow Based Programming -- FBP <https://en.wikipedia.org/wiki/Flow-based_programming#Concepts>`_. Here are some of the main **NiFi** concepts and how they map to **FBP**.

.. csv-table:: The core concepts of NiFi
   :header: "NiFi Term", "FBP Term", "	Description"
   :widths: 25, 25, 50

   "FlowFile", "Information Packet", "A FlowFile represents each object moving through the system and for each one, NiFi keeps track of a map of key/value pair attribute strings and its associated content of zero or more bytes"
   "FlowFile Processor", "Black Box", "Processors actually perform the work. In `eip <https://www.enterpriseintegrationpatterns.com/>`_ terms a processor is doing some combination of data routing, transformation, or mediation between systems. Processors have access to attributes of a given FlowFile and its content stream. Processors can operate on zero or more FlowFiles in a given unit of work and either commit that work or rollback"
   "Connection", "Bounded Buffer", "Connections provide the actual linkage between processors. These act as queues and allow various processes to interact at differing rates. These queues can be prioritized dynamically and can have upper bounds on load, which enable *back pressure*"
   "Flow Controller", "Scheduler", "The Flow Controller maintains the knowledge of how processes connect and manages the threads and allocations thereof which all processes use. The Flow Controller acts as the broker facilitating the exchange of FlowFiles between processors"
   "Process Group", "subnet", "A Process Group is a specific set of processes and their connections, which can receive data via input ports and send data out via output ports. In this manner, process groups allow creation of entirely new components simply by composition of other components"

This design model, also similar to `seda <https://www.mdw.la/papers/seda-sosp01.pdf>`_, provides many beneficial consequences that help NiFi to be a very effective platform for building powerful and scalable dataflows. A few of these benefits include:

+ Lends well to visual creation and management of directed graphs of processors;
+ Is inherently asynchronous which allows for very high throughput and natural buffering even as processing and flow rates fluctuate;
+ Provides a highly concurrent model without a developer having to worry about the typical complexities of concurrency;
+ Promotes the development of cohesive and loosely coupled components which can then be reused in other contexts and promotes testable units;
+ The resource constrained connections make critical functions such as *back-pressure* and *pressure release* very natural and intuitive;
+ Error handling becomes as natural as the happy-path rather than a coarse grained catch-all;
+ The points at which data enters and exits the system as well as how it flows through are well understood and easily tracked.

