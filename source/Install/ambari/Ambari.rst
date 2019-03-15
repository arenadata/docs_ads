Installing Arenadata Streaming with Ambari 2.7.0
==================================================

Installing the **Ambari 2.7.0** server goes through several steps:

+ `Setting up a remote YUM repository`_
+ `Install Ambari Server`_
+ `Ambari Server setup`_
+ `Start Ambari Server`_


Setting up a remote YUM repository
-------------------------------------

Setting up a remote repository is no different from setting up any additional repository *YUM*. To add a repository, you must run the following command as *root*:

  :command:`yum-config-manager –-add-repo http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.7.0.0/ambari.repo`


Install Ambari Server
-----------------------

The **Ambari** server is installed from the RPM package using the *YUM* command:

  :command:`yum install ambari-server`

The command sets the **Ambari** server which is the web application server to the port *8080*. Also installed is the server instance **PostgreSQL** on port *5432*.


Ambari Server setup
-----------------------

**Ambari** server must be configured to work correctly. If the instance **PostgreSQL** is configured on the default port you should run the following command:

  :command:`ambari-server setup`

During the setup process you must specify or accept the default settings:

+ *User account* -- you can choose any account to start the Ambari server (it is not necessary to log in from *root*). If the user does not exist, it is automatically created;
+ *Java JDK* -- to download **Oracle JDK 1.8**, you must enter the value *1* and accept the license **Oracle JDK** to download files from **Oracle**. The installation of **JDK** is done automatically;
+ *Database* -- select database:

  :command:`Enter advanced database configuration`

  At the command line you must answer *n* or *y*:

    + *n* -- for use with Ambari standard built-in database PostgreSQL. By default, the database name PostgreSQL is set to *ambari* and the login/password is *ambari/bigdata*;

    + *y* -- if you want to use with Ambari an existing PostgreSQL database, MySQL or Oracle instead of the default one. Next, for the selected database you must specify the connection parameters.



Start Ambari Server
----------------------

After installing the **Ambari** server start is performed by the command:

  :command:`ambari-server start`

To check the server status use the command:

  :command:`ambari-server status`

To stop the server use the command:

  :command:`ambari-server stop`

**Ambari** server is available on port *8080*. By default it is set to the following account:

+  User: *admin*
+  Password: *admin*

.. important:: It is recommended to change the password after the first login

To enter the web interface **Ambari** you need to specify the server address in the address bar of the browser:

  :command:`http://<адрес сервера>:8080`

This requests a login and password.  After authorization opens the web interface **Ambari** (:numref:`Pic.%s.<ADS_install_welcom-to-ambari-before-config>`).

.. _ADS_install_welcom-to-ambari-before-config:

.. figure:: ../imgs/ADS_install_welcom-to-ambari-before-config.*
   :align: center

   Ambari web interface before cluster setup



