.. title:: Cisco Catalyst VLANs and trunking

VLANs and trunking
==================

VLAN
----

VLANs effectively separate a switch into several network segments.
Ports can be affected to a specific VLAN, and machines from other VLANs
can't access each other without going through a router.
Each VLAN is usually assigned its own subnet.

Using VLANs can improve performance by separating broadcast domains,
and also increase security by preventing certain ports from accessing
other networks.

Trunk
-----

Normally, each port of the switch is assigned to a specific VLAN.
It would thus take multiple links between switches to carry multiple VLANS.

Trunks bring an answer to that scalability problem by allowing transit
of multiple VLANs across one link. Several encapsulations exist,
but 802.1Q is the recommendation.

802.1Q adds 4 bytes to the regular ethernet frame, of which 12 bits identify the VLAN,
and 3 bits specify the frame priority for QoS.

Native VLAN
-----------

The native VLAN is the VLAN for which frames are **not** tagged by 802.1Q.
Switches on both sides of a trunk must agree on the native VLAN, otherwise
they'll send the frames to the wrong ports.

The switches will print out warnings when CDP discovers native VLAN mismatch.

Router on a stick
-----------------

Unless you have Layer3 switches, a router is needed to route traffic between
VLANs. A router on a stick is a router connected to a switch via a trunk, and
is used to route between VLANs.

Trunking modes
--------------

Each port is is one of 4 trunking modes. Ports in ``trunk`` or ``dynamic desirable``
modes will send out DTP (Dynamic Trunking Protocol) frames to negociate a trunk.

================= ============================================================
Port mode         Description
================= ============================================================
access            forced to operate as an access port
trunk             forced to operate as a trunk.
dynamic auto      will accept trunk negociation, but default as an access port
dynamic desirable will initiate the negociation of a trunk
================= ============================================================

Commands
--------

VLAN summary
~~~~~~~~~~~~

::

    SW2#show vlan brief

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
                                                    Fa1/0/46, Fa1/0/48, Gi1/0/1
                                                    Gi1/0/2, Gi1/0/3, Gi1/0/4
    100  ADMIN                            active
    200  PRIV                             active
    300  SERV                             active

    VLAN Name                             Status    Ports
    ---- -------------------------------- --------- -------------------------------
    1002 fddi-default                     act/unsup
    1003 token-ring-default               act/unsup
    1004 fddinet-default                  act/unsup
    1005 trnet-default                    act/unsup

Set VLAN name
~~~~~~~~~~~~~

::

    SW2(config)#vlan 100
    SW2(config-vlan)#name ADMIN

Delete a VLAN
~~~~~~~~~~~~~

::

    SW2(config)#no vlan 100

Note that the VLANs will not be deleted if you erase the configuration of the switch.
They are store in a separate file, ``vlan.dat``, within the flash memory.

::

    SW2#show flash

    Directory of flash:/

        2  -rwx        2408   Mar 1 1993 02:58:12 +00:00  config.text
        3  -rwx           5   Mar 1 1993 02:58:12 +00:00  private-config.text
        4  -rwx         736   Mar 1 1993 01:07:47 +00:00  vlan.dat
        5  drwx         192   Mar 1 1993 00:05:52 +00:00  c3750-ipbase-mz.122-25.SEB2

    15998976 bytes total (8887296 bytes free)

If you want to fully erase the switch, including VLANs, you need to issue two commands.

::

    SW2#write erase
    SW2#delete flash:vlan.dat

Set the VLAN for a (range of) port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    SW2(config)#int fa 1/0/4
    SW2(config-if)#switchport access vlan 100

Or you can select a port range instead

::

    SW2(config)#int range fa 1/0/1 - 16
    SW2(config-if-range)#switchport access vlan 100

Show trunking summary
~~~~~~~~~~~~~~~~~~~~~

::

    SW2>show int trunk

    Port        Mode         Encapsulation  Status        Native vlan
    Fa1/0/47    desirable    802.1q         trunking      400

    Port      Vlans allowed on trunk
    Fa1/0/47    1-4094

    Port        Vlans allowed and active in management domain
    Fa1/0/47    1,100,200,300

    Port        Vlans in spanning tree forwarding state and not pruned
    Fa1/0/47    100,200,300


Show trunking status for a port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    SW2>show int fa 1/0/47 switchport
    Name: Fa1/0/47
    Switchport: Enabled
    Administrative Mode: dynamic desirable
    Operational Mode: trunk
    Administrative Trunking Encapsulation: dot1q
    Operational Trunking Encapsulation: dot1q
    Negotiation of Trunking: On
    Access Mode VLAN: 1 (default)
    Trunking Native Mode VLAN: 400 (Inactive)
    Administrative Native VLAN tagging: enabled
    Voice VLAN: none
    Administrative private-vlan host-association: none
    Administrative private-vlan mapping: none
    Administrative private-vlan trunk native VLAN: none
    Administrative private-vlan trunk Native VLAN tagging: enabled
    Administrative private-vlan trunk encapsulation: dot1q
    Administrative private-vlan trunk normal VLANs: none
    Administrative private-vlan trunk private VLANs: none
    Operational private-vlan: none
    Trunking VLANs Enabled: ALL
    Pruning VLANs Enabled: 2-1001
    Capture Mode Disabled
    Capture VLANs Allowed: ALL

    Protected: false
    Unknown unicast blocked: disabled
    Unknown multicast blocked: disabled
    Appliance trust: none

Change the encapsulation for a trunk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    SW2(config-if)#switchport trunk encapsulation dot1q

Change the native vlan for a trunk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    SW2(config-if)#switchport trunk native vlan 100

Change the trunking mode for a port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    SW2(config-if)#switchport mode dynamic desirable
