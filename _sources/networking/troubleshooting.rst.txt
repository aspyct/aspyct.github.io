.. title:: Network troubleshooting

Network troubleshooting
=======================

Wireshark
---------

Wireshark is the essential network analyzer.

Capture through SSH
~~~~~~~~~~~~~~~~~~~

::

  $ ssh root@192.168.91.1 tcpdump -i eth0 -U -s0 -w - | wireshark -k -i -

Packet captures
---------------

Instant TCP RST in repsonse to SYN
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. table::
   :widths: 20, 80

   ======== ===============================================================================
   pcap     `download </networking/troubleshooting/firewall-tcp-rst.pcapng>`_
   symptom  Impossible to establish a TCP connection. A RST reply is received very quickly.
   issue    Firewall was configured to reject this connection.
   solution Allow the necessary port in the firewall, if appropriate.
   ======== ===============================================================================

