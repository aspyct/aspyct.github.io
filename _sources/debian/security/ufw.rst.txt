ufw
===

The Uncomplicated FireWall is just what it is: an uncomplicated firewall.
Unless you need the full power of iptables for routing, nat etc, UFW is
a good option.

1. Install UFW

::

    $ sudo apt-get update
    $ sudo apt-get install ufw

2. Allow SSH

This will allow port 22 on IPv4 and IPv6. If you've modified your
``sshd_config`` to change the port, you might want to set that up
differently.

::

    $ sudo ufw allow ssh

3. Enable the firewall

::

    $ sudo ufw enable
    $ sudo ufw status
