Networking
==========

I just added a USB network interface, but no IP address is assigned
-------------------------------------------------------------------

You need to run ``dhclient`` on the interface.

::

    $ sudo dhclient <iface>

