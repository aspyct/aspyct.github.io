.. title:: OpenWRT unbound configuration

Unbound configuration
=====================

Add custom host entry
---------------------

Edit the file at ``/etc/unbound/unbound_srv.conf`` and add one line per host:

::

   local-data: "myserver.local IN A 192.168.0.4"

