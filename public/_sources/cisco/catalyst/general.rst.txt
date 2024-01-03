.. title:: Cisco Catalyst commands introduction

Commands intro
==============

Modes
-----

Normal mode ``Switch>``
    Read-only access to some of the switch info.

    - ``enable`` to go into priviledged mode.

Priviledged mode ``Switch#``
    User is allowed to configure the switch.

    - ``disable`` to go back to normal mode.
    - ``config terminal`` to go to configuration mode.

Configuration mode ``Switch(config)#``
    General configuration of the switch.

    - ``line ...`` to go to line configuration mode.
    - ``interface ...`` to go to interface configuration mode.

Line conf mode ``Switch(config-line)#``
    Configuration of the command lines like the console port CON or the VTYs.

Interface conf mode ``Switch(config-if)#``
    Configure a specific interface. It can be a lot of things, including:

    - a port, ``fa1/0/1``
    - a vlan, ``vlan 1``
    - etc.

Configure administrative access
-------------------------------

Set the hostname
~~~~~~~~~~~~~~~~

::

    Switch(config)# hostname SW-2.4

Assign a management IP address
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

From user mode,

::

    Switch> enable
    Switch# conf t
    Switch(config)# int vlan1
    Switch(config-if)# ip address 192.168.1.51 255.255.255.0
    Switch(config-if)# no shutdown

You might want to assign a default gateway as well.

Set the default gateway
~~~~~~~~~~~~~~~~~~~~~~~

::

    Switch(config)# ip default-gateway 192.168.1.1
    Switch(config)# end

Set the console password
~~~~~~~~~~~~~~~~~~~~~~~~

::

    Switch(config)# line con 0
    Switch(config-line)# password cisco
    Switch(config-line)# login
    Switch(config-line)# exit
    Switch(config)# line vty 0 15
    Switch(config-line)# password cisco
    Switch(config-line)# login
    Switch(config-line)# end
    Switch# exit

The console, "normal mode" password is now set to "cisco". The ``login`` command enables the password prompt.

By default, this password will also apply to access the priviledged mode,
but you can set a priviledged-specific password with ``(config)#enable secret``.

Set the priviledged mode password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    SW2(config)#enable secret mypassword


In addition to the console login password, you may set an administrator password
that will be required when entering the priviledged mode.

The secret is useful because it will not show as clear text in the ``show run | i enable``
command.

::

    SW2#show run | i enable
    enable secret 5 $1$Lnz4$CiqdpdGC45AFOXgcUJnbt/

5 is the level of hashing. Some switches support better hashing with PBKDF2 or SCrypt.

Set the inactivity timeout
~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    SW2(config)#line con 0
    SW2(config-line)#exec-timeout 2 30
    SW2(config-line)#exit
    SW2(config)#line vty 0 15
    SW2(config-line)#exec-timeout 2 30
    SW2(config-line)#exit

You'll be logged out after 2 minutes and 30 seconds from console or VTY.

In a lab, it's sometimes useful to disable timeout altogether.

::

    SW2(config)#line con 0
    SW2(config-line)#exec-timeout 0
    SW2(config-line)#exit

It's considered bad practice in a production environment for security purposes, though.

Enable password encryption
~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    SW2(config)#service password-encryption
    SW2#show run | i password
    service password-encryption
     password 7 060506324F41440A1245

.. WARNING::

    This will enable password mode "7", which is super weak
    and can be readily cracked on `some websites <http://www.ifm.net.nz/cookbooks/passwordcracker.html>`_.
    It will only prevent over-the-shoulder password theft.

Change the login banner
~~~~~~~~~~~~~~~~~~~~~~~

::

    SW2(config)#banner login ;
    Enter TEXT message.  End with the character ';'.
    Hey you're not supposed to be in here.
    ;

Configure Telnet
~~~~~~~~~~~~~~~~

**Don't**.

But if you really must (like me, since my old cisco switches don't support SSH)...

::

    Switch(config)# line vty 0 15
    Switch(config-line)# transport input telnet
    Swicth(config-line)# end

Basic port configuration
------------------------

Configure port speed and duplex
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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

Save the configuration
----------------------

By default, the running configuration will be lost when the switch reboots.
We need to store the running configuration into the startup config's NVRAM (non-volatile ram).

::

    SW2#copy running-config startup-config
    Destination filename [startup-config]?
    Building configuration...
    [OK]

.. NOTE::
    You can also simple run ``wr``, but this is deprecated.