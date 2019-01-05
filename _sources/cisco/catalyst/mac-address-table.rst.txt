.. title:: Cisco Catalyst mac address table commands

Mac Address Table
=================

Show the current mac address table
----------------------------------

::

    SW1>show mac-address-table
            Mac Address Table
    -------------------------------------------

    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----
    All    0100.0ccc.cccc    STATIC      CPU
    All    0100.0ccc.cccd    STATIC      CPU
    All    0180.c200.0000    STATIC      CPU
    All    0180.c200.0001    STATIC      CPU
    All    0180.c200.0002    STATIC      CPU
    All    0180.c200.0003    STATIC      CPU
    All    0180.c200.0004    STATIC      CPU
    All    0180.c200.0005    STATIC      CPU
    All    0180.c200.0006    STATIC      CPU
    All    0180.c200.0007    STATIC      CPU
    All    0180.c200.0008    STATIC      CPU
    All    0180.c200.0009    STATIC      CPU
    All    0180.c200.000a    STATIC      CPU
    All    0180.c200.000b    STATIC      CPU
    All    0180.c200.000c    STATIC      CPU
    All    0180.c200.000d    STATIC      CPU
    All    0180.c200.000e    STATIC      CPU
    All    0180.c200.000f    STATIC      CPU
    All    0180.c200.0010    STATIC      CPU
    All    ffff.ffff.ffff    STATIC      CPU
    1    0015.2b35.8644    DYNAMIC     Fa1/0/47
    1    0015.2b3e.3d72    DYNAMIC     Fa1/0/20
    1    e276.6A72.1f13    DYNAMIC     Fa1/0/1
    1    f054.f139.eb6a    DYNAMIC     Fa1/0/1
    Total Mac Addresses for this criterion: 24

The **dynamic** addresses have been learned from the network traffic,
while the **static** ones are configured either by default or by the
administrator.

The address ``0100.0ccc.cccc`` is the Cisco Discovery Protocol (CDP) address.
Other static/CPU addresses are special mac addresses that are used by the switch.

Statically configure a mac address
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    SW1(config)#mac address-table static a820.6632.0087 vlan 1 interface fa1/0/1

This will statically configure the mac address a820... to be in (or from?) VLAN 1 on interface fa1/0/1.

Configure mac aging
~~~~~~~~~~~~~~~~~~~

::

    SW1(config)#mac-address-table aging-time 300

This will set the mac address aging to 5 minutes.
If a mac address has not been seen during the last 5 minutes,
it will be discarded from the table.
