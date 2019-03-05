Installing ADS cluster with ADCM
=================================

The simplest way to install an **ADS** cluster is to use an **Arenadata Cluster Manager** -- `ADCM <https://arenadata.tech/en/products/adcm/>`_.



Benefits
------------

The advantages of installing the **ADS** cluster through the **ADCM** are:

1. All installation and configuration logic is encapsulated in the ADS bundle:

+ User doesn't have to enter a large number of console commands;
+ All important settings of OS and database are configured.

2. Single interface for access and configuration management:

+ Ability to manage multiple services through a single interface
+ Ability to manage multiple clusters through a single interface

3. There is a possibility to use both cloud and physical infrastructure.

4. All ADS rpm packets are taken from the Arenadata repository:

+ All rpms have been tested;
+ A fixed set of packages is used which simplifies user assistance and compiling bug reports.



.. _preliminary_actions:

Preliminary actions
-------------------------

To install the **ADS** cluster using **ADCM** you must complete the following preliminary steps:

1. Install ADCM;
2. Create the hosts for the ADS cluster:

+ Load required host bundle. The current example uses the *Datafort* bundle;
+ Initialize the required number of hosts (in the current example, this step is skipped because the hosts are ready, not the cloud provider);
+ Add hosts to ADCM (the current example uses 4 hosts): for Zookeeper (*zk*), for Kafka brokers (*kafka 1* and *kafka2*), for Nifi (*nifi1*);

3. (Optional) Create the monitoring cluster:

+ Load monitoring bundle;
+ Instantiate and install monitoring cluster.


Installation steps
-------------------

.. toctree::
   :maxdepth: 2

   upload_ads_bundle/index
   create_cluster/index
   manage_tools/index


