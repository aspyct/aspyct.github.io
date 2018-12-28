Spanning Tree Protocol (STP)
============================

Summary
-------

The Spanning Tree Protocol (STP for short) is a protocol used by switches
to avoid switching loops, while still providing link redundancy in the network.

They do this by electing a root switch and defining which link, for each switch,
is the shortest path to the root switch. Ports are then selectively disabled,
and the network graph is turned into a tree, which is by definition loop-free.

.. NOTE::
    The terms "switch" and "bridges" both refer to the same devices.

What STP solves
~~~~~~~~~~~~~~~

- broadcast storms
- corrupted MAC address tables
- duplicate packet deliveries

Root selection criteria
~~~~~~~~~~~~~~~~~~~~~~~

In order:

1. switch priority (default = 32768)
2. lowest MAC address

Port roles
~~~~~~~~~~

============== ==================================================================================
Role           Explanation
============== ==================================================================================
root           the port on a non-root bridge that is closest to the root bridge, in terms of cost
designated     the port on a network segment that is closest to the root bridge, in terms of cost
non-designated ports that block traffic, in order to preserve a loop-free Layer 2 topology
disabled       a port that is administratively shut down
============== ==================================================================================

.. NOTE::
    A "network segment" is any non-switched link between two switches.
    The most simple network segment you can find is a cable connection between two switches.

Default port cost
~~~~~~~~~~~~~~~~~

======== ======== ========
Speed    Old cost New cost
======== ======== ========
10 Mbps  100
100 Mbps 19
1 Gbps   4
10 Gbps  2
======== ======== ========

.. NOTE::
    When selecting the root port on a switch, if two ports have the same cost to root,
    then the tie breaker is the **remote** switch's port priority.

STP convergence times
~~~~~~~~~~~~~~~~~~~~~

If a used link goes down, it takes 50 seconds for a blocking link to start forwarding again
(with the old 802.1d STP variety).

========== ==========================
Port state Minimum time to next state
========== ==========================
Blocking   20s
Listening  15s
Learning   15s
Forwarding
========== ==========================

Rapid Spanning Tree Protocol greatly reduces the time it takes to recover from a broken link.

STP Flavors
-----------

============ ==============================================
Abbreviation Description
============ ==============================================
STP          The "Common" Spanning Tree as explained above.
PVST+        Per VLAN Spanning Tree
MSTP         Multiple Spanning Trees Protocol,
             sometimes referred to as MST.

             Similar to PVST+ except that multiple VLANs
             are assigned the same root.
Rapid PVST+  Modified version of PVST+ that takes roughly
             3s to converge.
============ ==============================================

Rapid PVST+
-----------

Synchronization
~~~~~~~~~~~~~~~

Rapid STP synchronization is a 5 step process between two switches.
It is triggered whenever a switch, A, gets a new Root port.

1. Switch A blocks ports that are on the opposite side of designated port from switch B
2. Switch A sends a Proposal for the new route to root to switch B
3. If this is a new best route to root for switch B, the port will change from designated port
   to root port.
4. Switch B will send an Agreement back to switch A
5. Switch A changes its port state from Blocking to Forwarding

This process cascades down the switch chain. At step 3, switch B will also start the process
as switch A did during step 1.

Port roles
~~~~~~~~~~

Instead of non-designated ports, we have **alternate** and **backup** ports.

alternate port
    An alternate port can reach the root but is not the lowest cost port to it.

backup port
    A backup port exists when we have more than one port going from a bridge to a shared
    media (eg. a hub). In that case, only one of those ports will be a designated port,
    and the other ones will be backup ports.

    A backup port is blocking.

Port states
~~~~~~~~~~~

========== ===============================================================
State      Description
========== ===============================================================
Discarding data is not being forwarded on the port.
           (seen on Alternate, Backup and Disabled ports)
Learning   the switch is learning MAC addresses available off of the port.
           (seen when a port is transitioning to Forwarding)
Forwarding data is being forwarded on the port.
           (seen on Root and Designated ports)
========== ===============================================================

Link types
~~~~~~~~~~

============== =====================================================
Type           Description
============== =====================================================
Point-to-Point a link between two switches
Shared         a link to a hub or other shared medium
Edge Port      a direct link to a computer or other endpoint machine
============== =====================================================

Commands
--------

``show spanning-tree vlan 100``

    Show STP information for the VLAN 100.

    - who's the root bridge, what's its priority and address, hello time, max age and forward delay
    - same info about the bridge you're on
    - role, state, cost, priority and type for each port of this bridge

``int fa 0/1``

    Select the port "fa 0/1"

``spanning-tree port-priority 192``

    Change the priority to 192 for the selected port.

``spanning-tree cost 18``

    Change the cost to 18 for the selected port.

External links
--------------

- https://www.youtube.com/watch?v=0tlrQC2uJN4