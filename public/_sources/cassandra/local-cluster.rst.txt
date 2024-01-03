DIY Local Cassandra cluster
===========================

It's easy to run a local cassandra cluster with docker,
provided that you have enough memory on your system.

The standard docker container will consume 4G of memory,
but you can easily reduce that down to 1G with the following Dockerfile.

::

   FROM cassandra

   RUN sed -i'' 's/#-Xms4G/-Xms1G/' /etc/cassandra/jvm.options 
   RUN sed -i'' 's/#-Xmx4G/-Xmx1G/' /etc/cassandra/jvm.options

Build the container and tag it as ``small-cassandra:latest``.

::

   $ sudo docker build . -t 'small-cassandra:latest'

Then start the first node of your cluster.

::

   $ sudo docker run --name cassandra1 -d small-cassandra

And add nodes at will. Make sure to change the name each time.

::
   
   $ sudo docker run --name cassandra2 -d --link cassandra1:cassandra small-cassandra

Allow ample time for the cluster to settle.
You can get the cluster status with ``nodetool status``

::

   $ sudo docker exec -it cassandra1 bash
   root@497929a901a9:/# nodetool status
   Datacenter: datacenter1
   =======================
   Status=Up/Down
   |/ State=Normal/Leaving/Joining/Moving
   --  Address     Load       Tokens       Owns (effective)  Host ID                               Rack
   UN  172.17.0.3  92.5 KiB   256          100.0%            0e0d319f-4767-4a81-9632-45b8cc0ed441  rack1
   UN  172.17.0.2  108.63 KiB  256          100.0%            ebc07dc6-9c35-4ab6-9799-4be9e8358aff  rack1

