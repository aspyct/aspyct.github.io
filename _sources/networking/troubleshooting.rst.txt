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

.. contents::
   :local:

Instant TCP RST in repsonse to SYN
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Symptom (`pcap </networking/troubleshooting/firewall-tcp-rst.pcapng>`_)
    Impossible to establish a TCP connection. A RST reply is received very quickly.

Diagnostic
    Capture packets on various interfaces:
     
    1. workstation output
    2. firewall lan input
    3. firewall wan output
   
    The packet was visible on 1 and 2, but never on 3.

Issue
    The firewall was configured to reject this connection.

Solution
    Allow the necessary port in the firewall, if appropriate.

