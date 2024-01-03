Unattended upgrades
===================

Quickstart
----------

::

    $ sudo apt-get install unattended-upgrades apt-listchanges



In ``/etc/apt/apt.conf.d/50unattended-upgrades``, uncomment or modify the following lines:

::

    Unattended-Upgrade::Mail "root";
    Unattended-Upgrade::Remove-Unused-Dependencies "true";

You may also chose automatic reboot, but if your server is encrypted, it
won't reboot properly.

Make sure that ``/etc/apt/apt.conf.d/20auto-upgrades`` contains the
following lines:

::

    APT::Periodic::Update-Package-Lists "1";
    APT::Periodic::Unattended-Upgrade "1";

Finally, test that everything works properly:

::

    $ sudo unattended-upgrade -d


External links
--------------
- `Full guide <https://wiki.debian.org/UnattendedUpgrades>`_