.. title:: Cisco Catalyst basic troubleshooting

Basic troubleshooting
=====================

Show interfaces
---------------

::

    >show interfaces status

    Port      Name               Status       Vlan       Duplex  Speed Type
    Fa1/0/1                      notconnect   1            auto   auto 10/100BaseTX
    Fa1/0/2                      notconnect   1            auto   auto 10/100BaseTX
    Fa1/0/3                      notconnect   1            auto   auto 10/100BaseTX
    Fa1/0/4                      notconnect   1            auto   auto 10/100BaseTX
    ...
    Fa1/0/47                     notconnect   1            auto   auto 10/100BaseTX
    Fa1/0/48                     connected    1          a-full  a-100 10/100BaseTX
    Gi1/0/1                      notconnect   1            auto   auto Not Present
    Gi1/0/2                      notconnect   1            auto   auto Not Present
    Gi1/0/3                      notconnect   1            auto   auto Not Present
    Gi1/0/4                      notconnect   1            auto   auto Not Present


::

    >show interfaces fa1/0/48
    FastEthernet1/0/48 is up, line protocol is up (connected)
        Hardware is Fast Ethernet, address is 0015.2b2e.2634 (bia 0015.2b2e.2634)
        MTU 1500 bytes, BW 100000 Kbit, DLY 100 usec,
            reliability 255/255, txload 1/255, rxload 1/255
        Encapsulation ARPA, loopback not set
        Keepalive set (10 sec)
        Full-duplex, 100Mb/s, media type is 10/100BaseTX
        input flow-control is off, output flow-control is unsupported
        ARP type: ARPA, ARP Timeout 04:00:00
        Last input 00:00:12, output 00:00:00, output hang never
        Last clearing of "show interface" counters never
        Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
        Queueing strategy: fifo
        Output queue: 0/40 (size/max)
        5 minute input rate 0 bits/sec, 0 packets/sec
        5 minute output rate 0 bits/sec, 0 packets/sec
            72 packets input, 6636 bytes, 0 no buffer
            Received 52 broadcasts (0 multicast)
            0 runts, 0 giants, 0 throttles
            0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
            0 watchdog, 18 multicast, 0 pause input
            0 input packets with dribble condition detected
            155 packets output, 23576 bytes, 0 underruns
            0 output errors, 0 collisions, 1 interface resets
            0 babbles, 0 late collision, 0 deferred
            0 lost carrier, 0 no carrier, 0 PAUSE output
            0 output buffer failures, 0 output buffers swapped out

The first line indicates the layer1 and layer2 status of the interface.

==================== ================= =============================================================================
Layer 1              Layer 2           Status
==================== ================= =============================================================================
up                   up                Ok
up                   down              Connection issues, not receiving the L2 keepalive
down                 down (notconnect) Cable is not connected to at least one switch, or other switch port disabled.
down                 down              Layer 1 issue or other switch port administratively shut down.
adinistratively down down              Interface administratively shut down
==================== ================= =============================================================================

The last block gives statistics on the packets.

=============== ==============================================================
Parameter       Description
=============== ==============================================================
runts           Frame too small and bad CRC (<46 bytes for ethernet)
giants          Frame too big and bad CRC (>1518 bytes for non-jumbo ethernet)
input errors    Sum of input errors (runts, giants, CRC etc)
output errors   Sum of packets that haven't been transmitted because of errors
collisions      Number of collisions that occurred (in half duplex mode)
late collisions Number of collisions that happened after the slot time
CRC             Incorrect CRC. CRC is a type of FCS
=============== ==============================================================

Common errors
-------------

Duplex mismatch
~~~~~~~~~~~~~~~

It's easy to get a duplex mismatch, if one swith is trying to autonegociate
the duplex mode and the other switch is fixed either to half or full duplex.

One of the tell-tale symptoms is having lots of CRC errors on the full-duplex side,
and lots of late collisions on the half-duplex side.

Clear counters
--------------

After solving (or trying to solve) an issue, clear the error counters.

::

    #clear counters
    Clear "show interface" counters on all interfaces [confirm]
    00:31:40: %CLEAR-5-COUNTERS: Clear counter on all interfaces by console

VLAN troubleshooting
--------------------

::

    >show vlan brief

    VLAN Name                             Status    Ports
    ---- -------------------------------- --------- -------------------------------
    1    default                          active    Fa1/0/1, Fa1/0/2, Fa1/0/3
                                                    Fa1/0/4, Fa1/0/5, Fa1/0/6
                                                    Fa1/0/7, Fa1/0/8, Fa1/0/9
                                                    Fa1/0/10, Fa1/0/11, Fa1/0/12
                                                    Fa1/0/13, Fa1/0/14, Fa1/0/15
                                                    Fa1/0/16, Fa1/0/17, Fa1/0/18
                                                    Fa1/0/19, Fa1/0/20, Fa1/0/21
                                                    Fa1/0/22, Fa1/0/23, Fa1/0/24
                                                    Fa1/0/25, Fa1/0/26, Fa1/0/27
                                                    Fa1/0/28, Fa1/0/29, Fa1/0/30
                                                    Fa1/0/31, Fa1/0/32, Fa1/0/33
                                                    Fa1/0/34, Fa1/0/35, Fa1/0/36
                                                    Fa1/0/37, Fa1/0/38, Fa1/0/39
                                                    Fa1/0/40, Fa1/0/41, Fa1/0/42
                                                    Fa1/0/43, Fa1/0/44, Fa1/0/45
                                                    Fa1/0/46, Fa1/0/47, Fa1/0/48
                                                    Gi1/0/1, Gi1/0/2, Gi1/0/3
                                                    Gi1/0/4
    100  ADMIN                            active
    1002 fddi-default                     act/unsup
    1003 token-ring-default               act/unsup
    1004 fddinet-default                  act/unsup
    1005 trnet-default                    act/unsup

In the example above, the VLAN 100 exists, but no port is assigned.
That could be the sign of an error if some machines can't communicate together.

If you delete a VLAN that has ports assigned to it, the ports are no longer assigned
to any VLAN and they're not going to be able to communicate.

Before deleting a VLAN, make sure you remove any port assigned to this VLAN.
