Preparing to install Arenadata Streaming MPack
==================================================

The main components **ADS** are installed using **Ambari Management Pack** (**MPack**):

+ `Download Arenadata Streaming MPack`_
+ `Import MPack to Ambari stack`_


Download Arenadata Streaming MPack
------------------------------------

To install **Arenadata Streaming MPack**, you need to download and unpack the following archives in a dedicated place (you should avoid using the */tmp* directory):

+ *ads-ambari-mpack-{version}.tar.gz* -- a set of service and script configurations for deploying and configuring the platform via **Ambari**.

If archives are loaded into the */tmp* directory then to unpack them in a directory for example */staging* you need to run the following command:

  :command:`tar –xzvf /tmp/ads-ambari-mpack-{version}.tar.gz -C /staging/`



Import MPack to Ambari stack
----------------------------------

To import the **ADS** stack into **Ambari** you need to run the following commands:

1. Save the current folder *Ambari Resources* for possible system recovery:

  :command:`cp -r /var/lib/ambari-server/resources /var/lib/ambari-server/resources.backup`

2. Run the stack import command:

  :command:`ambari-server install-mpack --mpack=/staging/ads-ambari-mpack-{version}.tar.gz --purge --verbose`

3. Restart **Ambari**:

  :command:`ambari-server restart`
