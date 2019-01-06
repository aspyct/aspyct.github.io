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
