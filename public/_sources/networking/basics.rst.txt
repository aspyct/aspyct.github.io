Networking Basics
=================

Layers
------

+---+--------------+--------------+
|   | OSI          | TCP/IP Stack |
+---+--------------+--------------+
| 7 | Application  |              |
+---+--------------+              |
| 6 | Presentation | Application  |
+---+--------------+              |
| 5 | Session      |              |
+---+--------------+--------------+
| 4 | Transport    | Transport    |
+---+--------------+--------------+
| 3 | Network      | Internet     |
+---+--------------+--------------+
| 2 | Data Link    | Link         |
+---+--------------+--------------+
| 1 | Physical     | Physical     |
+---+--------------+--------------+

Protocol Data Units (PDUs)
--------------------------

============ =======
Layer        PDU
============ =======
4. Transport Segment
3. Internet  Packet
2. Link      Frame
1. Physical  Bit
============ =======

Network topologies
------------------

Star topology "central switch"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- if one link fails, the other links continue to function
- central device is a SPOF
- popular in modern networks

Mesh topology "multiple offices"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Allows for redundancy between each enpoint. In a full mesh, you need
(n * (n - 1) / 2) links. Therefore, full mesh networks don't scale well
when the number of endpoints increases.

Links that are less useful can be removed. The network becomes a partial mesh.
Endpoints can still reach each other by routing their traffic through other endpoints.

============== ========================
Full mesh      Partial mesh
============== ========================
Optimal path   Might be suboptimal path
Not Scalable   More Scalable
More expensive Less expensive
============== ========================


Network architecture
--------------------

Three Tiered
~~~~~~~~~~~~

================== ====================================================================
Layer              Description
================== ====================================================================
Access Layer       Connected to endpoints
Distribution Layer Distributes the network to access layer
Core Layer         Connect several distribution layers together (buildings, BUs etc...)
================== ====================================================================

Collapsed Core
~~~~~~~~~~~~~~

A collapsed core network is basically a three tiered network with the Distribution Layer
and Core Layer merged into the so-called "Collapsed Core Layer".

Cables and connectors
---------------------

Copper
~~~~~~

Nowadays we typically use Unshielded Twisted Pair cables with RJ45 connectors.

.. table:: Copper cables

    ==== ============ ==============================================
    Name Type         Notes
    ==== ============ ==============================================
    RG59 Coax         lossy, for short distances only
    RG6  Coax         typical TV cable
    RG58 Coax         50 Ohms, used for 10 Base 2
    UTP  Twisted pair unshielded twisted pair, typical cable today
    STP  Twisted pair shielded twisted pair, more expensive than UTP
    ==== ============ ==============================================

.. table:: Copper connectors

    =========== ============ =========================
    Name        Type         Notes
    =========== ============ =========================
    F Connector Coax
    BNC         Coax         Used with 10 Base 2
    DB9         Serial       Often used for management
    RJ45/RJ11   Twisted pair Most common today
    =========== ============ =========================

.. table:: Twisted pair cable categories

    ======== =================================================
    Category Ethernet
    ======== =================================================
    Cat 3    10 Base T
    Cat 5    100 Base TX
    Cat 5e   1000 Base T
    Cat 6    1000 Base T (thicker cables, better transmission)
    Cat 6a   10G Base T
    ======== =================================================

.. table:: RJ45 wiring pinout. Pin 1 is left when holding the cable with the security clip down.

    +-----+--------------+--------------+
    | Pin | T568A        | T568B        |
    +-----+--------------+--------------+
    | 1   | white/green  | white/orange |
    +-----+--------------+--------------+
    | 2   | green        | orange       |
    +-----+--------------+--------------+
    | 3   | white/orange | white/green  |
    +-----+--------------+--------------+
    | 4   | blue                        |
    +-----+-----------------------------+
    | 5   | white/blue                  |
    +-----+--------------+--------------+
    | 6   | orange       | green        |
    +-----+--------------+--------------+
    | 7   | white/brown                 |
    +-----+-----------------------------+
    | 8   | brown                       |
    +-----+-----------------------------+

Fiber optic
~~~~~~~~~~~

- Immune to electromagnetic interference
- Can cover much larger distances than copper
- LED or Laser (more powerful)

.. table:: Fiber optic cables

    =========== ============================================
    Type        Notes
    =========== ============================================
    single mode one ray of light, up to 40km. More expensive
    multimode   modal dispersion limits to ~2km
    =========== ============================================

.. table:: Fiber optic connectors

    ==== ===================================
    Type Notes
    ==== ===================================
    ST
    LC
    SC
    MTRJ 2 strands of fiber in one connector
    ==== ===================================

Troubleshooting
---------------

Approaches
~~~~~~~~~~

================== =============================================================
Name               Description
================== =============================================================
Top-down           Start at the app level, eg. can I reach website from browser?
Bottom-up          Start at the bottom, eg. is the cable plugged in?
Divide and Conquer Start in the middle and bisect, eg. start with ping
================== =============================================================

Steps
~~~~~

1. Define the problem in a specific way
2. Collect information
3. Analyze information
4. Eleminate unlikely causes
5. Propose a hypothesis
6. Test hypothesis (don't interrupt people/systems if at all possible)
7. Solve and document

If it didn't work, start over at 2.

External links
--------------

- `Star Topology on Wikipedia <https://en.wikipedia.org/wiki/Star_network>`_
- `Mesh Topology on Wikipedia <https://en.wikipedia.org/wiki/Mesh_networking>`_
- `Three Tiered network <http://www.omnisecu.com/cisco-certified-network-associate-ccna/three-tier-hierarchical-network-model.php>`_