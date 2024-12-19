.. title:: Cisco Catalyst port configuration

Basic port configuration
========================

Speed and duplex
----------------

It can be useful to configure half-duplex if you're connecting to some old tech, like a hub.
For the user facing ports, you will probably prefer auto speed for these ports.
However, autoconfiguration does not alway work within the infrastructure,
and you may prefer to set link speed manually.

::

    SW2(config)#int fa 1/0/30
    SW2(config-if)#speed ?
    10    Force 10 Mbps operation
    100   Force 100 Mbps operation
    auto  Enable AUTO speed configuration

    SW2(config-if)#speed auto

::

    SW2(config-if)#duplex ?
    auto  Enable AUTO duplex configuration
    full  Force full duplex operation
    half  Force half-duplex operation

    SW2(config-if)#duplex half


Another option worth knowing is MDIX. It's the mechanism that allows the switch
to know which twisted pair to use for transmission, and which for reception.
It's on auto by default, and can only be auto on my Catalyst 3750.

::

    SW2(config-if)#mdix ?
    auto  Enable automatic MDI crossover detection on this interface

    SW2(config-if)#mdix auto

Get info on a specific interface
--------------------------------

::

    SW2#show int fa 1/0/30
    FastEthernet1/0/30 is up, line protocol is up (connected)
    Hardware is Fast Ethernet, address is 0015.2b2e.3c22 (bia 0015.2b2e.3c22)
    MTU 1500 bytes, BW 100000 Kbit, DLY 100 usec,
        reliability 255/255, txload 1/255, rxload 1/255
    Encapsulation ARPA, loopback not set
    Keepalive set (10 sec)
    Full-duplex, 100Mb/s, media type is 10/100BaseTX
    input flow-control is off, output flow-control is unsupported
    ARP type: ARPA, ARP Timeout 04:00:00
    Last input 00:00:01, output 00:00:00, output hang never
    Last clearing of "show interface" counters never
    Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
    Queueing strategy: fifo
    Output queue: 0/40 (size/max)
    5 minute input rate 0 bits/sec, 0 packets/sec
    5 minute output rate 0 bits/sec, 0 packets/sec
        7041 packets input, 513625 bytes, 0 no buffer
        Received 5118 broadcasts (0 multicast)
        0 runts, 0 giants, 0 throttles
        0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
        0 watchdog, 5060 multicast, 0 pause input
        0 input packets with dribble condition detected
        2541 packets output, 256741 bytes, 0 underruns
        0 output errors, 0 collisions, 1 interface resets
        0 babbles, 0 late collision, 0 deferred
        0 lost carrier, 0 no carrier, 0 PAUSE output
        0 output buffer failures, 0 output buffers swapped out

This command also works with other interfaces like VLANs etc.
