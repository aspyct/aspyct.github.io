IP Routing overview
===================

Address classes
---------------

===== =========== =============================== ===============================
Class First octet Classful mask (dotted notation) Classful mask (prefix notation)
===== =========== =============================== ===============================
A     1 - 126     255.0.0.0                       /8
B     128 - 191   255.255.0.0                     /16
C     192 - 223   255.255.255.0                   /24
D     224 - 239   Multicast - no mask
E     240 - 255   Experimental - no mask
===== =========== =============================== ===============================

The class of an IP address is **entirely determined** by the first octet.
The default network mask is always the corresponding classful mask.

ICANN assigns IPv4 address blocks, and delegates the fine-grained attribution
to continental organisations.

Private address ranges
----------------------

The private address ranges are not routable on the public internet.
NAT is used to route from publicly addressable IP addresse to private addresses.

===== ============================= ======
Class Range                         Prefix
===== ============================= ======
A     10.0.0.0 - 10.255.255.255     /8
B     172.16.0.0 - 172.31.255.255   /16
B     169.254.0.0 - 169.254.255.255 /16
C     192.168.0.0 - 192.168.255.255 /24
===== ============================= ======

The 169.254.0.0 addresses are autoconfigured address, usually assigned when
no static IP or DHCP is available. They are APIPA addresse, and are not routable
by any router, even on local networks.

CIDR
----

"Classless Inter-Domain Routing" is aggregating several `subnets </networking/subnetting.html>`_
under one route. For example, the following four network can be addressed as one:

- 192.168.32.0/24
- 192.168.33.0/24
- 192.168.34.0/24
- 192.168.35.0/24

You only need 1 route advertisement for those 4 networks, ``192.168.32.0/22``.
This saves space on the routing tables.

Addressing methods
------------------

IPv4
~~~~

- **Unicast**: one-to-one, eg. HTTP
- **Broadcast**: one-to-all, eg. ARP
- **Multicast**: one-to-many-subscribers, eg. RIPv2, OSPF, EIGRP

IPv6
~~~~

- **Unicast**
- **Multicast**
- **Anycast**: select the closest corresponding host, eg. DNS Anycast

IPv6
----

128 bits per address.

Benefits of IPv6
~~~~~~~~~~~~~~~~

- Roughly 5 * 10^28 addresses available per person on the planet
- Simplified headers, 8 fields instead of 12
- Designed with security and device mobility in mind
- No broadcast (but certain types of multicast are equivalent)
- MTU discovery performed for each connection
- Can and does coexist with IPv4 (via dual stack or IPv6 over IPv4 tunneling)

Structure
~~~~~~~~~

Written in hexadecimal, 8 groups of 4 digits. They can be shortened:

1. the leading zeroes in a group can be omitted
2. the biggest continuous group of zero can be replaced with:

``2041:0001:140F:0000:0000:0000:875B:131B`` becomes ``2041:1:140F::875B:131B``

Global Unicast addresses
~~~~~~~~~~~~~~~~~~~~~~~~

Addresses starting with bits ``001``, in other terms addresses in the 2000::/3 network,
are the `global unicast addresses <https://www.iana.org/assignments/ipv6-unicast-address-assignments/ipv6-unicast-address-assignments.xhtml>`_.

====== ===================== ========= =============
001    global routing prefix subnet id interface id
3 bits 45 bits               16 bits   64 bits
====== ===================== ========= =============

Multicast
~~~~~~~~~

Addresses have a FF-valued first byte.

======== ====== ====== ========
11111111 flags  scope  group id
8 bits   4 bits 4 bits 112 bits
======== ====== ====== ========