.. title:: Cisco Catalyst spanning tree commands

Spanning Tree
=============

Show spanning tree info
~~~~~~~~~~~~~~~~~~~~~~~

::

    SW1>show spanning-tree

    VLAN0001
    Spanning tree enabled protocol ieee
    Root ID    Priority    16385
                Address     0015.2b2e.2600
                This bridge is the root
                Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

    Bridge ID  Priority    16385  (priority 16384 sys-id-ext 1)
                Address     0015.2b2e.2600
                Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
                Aging Time 150

    Interface        Role Sts Cost      Prio.Nbr Type
    ---------------- ---- --- --------- -------- --------------------------------
    Fa1/0/1          Desg FWD 19        128.3    P2p
    Fa1/0/20         Desg FWD 19        128.22   P2p
    Fa1/0/47         Desg FWD 19        128.51   P2p


Change spanning tree mode
~~~~~~~~~~~~~~~~~~~~~~~~~

::

    SW1(config)#spanning-tree mode rapid-pvst

Change port priority
~~~~~~~~~~~~~~~~~~~~

::

    SW1(config-if)#spanning-tree port-priority 128

Priority ranges from 0 to 240, by increments of 16. Default is 128.

Change port cost
~~~~~~~~~~~~~~~~

::

    SW1(config-if)#spanning-tree cost 18

Refer to the `link cost table </networking/spanning-tree-protocol.html#default-port-cost>`_
for more info on the default port cost.
