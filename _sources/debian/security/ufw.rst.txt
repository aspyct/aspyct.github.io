ufw
===

Quickstart
----------

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

Allow only certain interfaces
-----------------------------

It is possible to allow traffic on certain interfaces only, and in a certain direction.
For example, to allow SSH only on interface enp5s0:

::

    $ sudo ufw allow in on enp5s0 to any port 2 proto tcp


Delete a rule
-------------

To delete a rule, you need its number.

::

    $ sudo ufw status numbered
    Status: active

        To                         Action      From
        --                         ------      ----
    [ 1] 443/tcp                    ALLOW IN    Anywhere
    [ 2] 22/tcp on enx0050b697dd22  ALLOW IN    Anywhere
    [ 3] 80/tcp (v6)                ALLOW IN    Anywhere (v6)
    [ 4] 443/tcp (v6)               ALLOW IN    Anywhere (v6)
    [ 5] 22/tcp (v6) on enx0050b697dd22 ALLOW IN    Anywhere (v6)

    $ sudo ufw delete 3
    Deleting:
    allow 80/tcp
    Proceed with operation (y|n)? y
    Rule deleted (v6)

