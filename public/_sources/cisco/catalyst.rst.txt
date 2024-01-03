Catalyst switch
===============

Modes
-----

User mode
    The default mode when you enter management console.
    Nothing can be changed from that mode, but you can
    look at the config.

Priviledged mode
    This is the mode you need to change the configuration.

    Type ``enable`` to enter priviledged mode, ``disable`` to exit.

Commands
--------

Assign a management IP address
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

From user mode,

::

    Switch> enable
    Switch# conf t
    Switch(config)# int vlan1
    Switch(config-if)# ip address 192.168.1.51 255.255.255.0
    Switch(config-if)# no shutdown

Set the default gateway
~~~~~~~~~~~~~~~~~~~~~~~

From user mode,

::

    Switch> enable
    Switch# conf t
    Switch(config)# ip default-gateway 192.168.1.1
    Switch(config)# end

Set the management password
~~~~~~~~~~~~~~~~~~~~~~~~~~~

From priviledged mode,

::

    Switch# conf t
    Switch(config)# line con 0
    Switch(config-line)# password cisco
    Switch(config-line)# login
    Switch(config-line)# exit
    Switch(config)# line vty 0 15
    Switch(config-line)# password cisco
    Switch(config-line)# login
    Switch(config-line)# end
    Switch# exit

The password is now set to "cisco". The ``login`` command enables the password prompt.

Configure Telnet
~~~~~~~~~~~~~~~~

**Don't**.

But if you really must (like me, since my old cisco switches don't support SSH)...

::

    Switch# conf t
    Switch(config)# line vty 0 15
    Switch(config-line)# transport input telnet
    Swicth(config-line)# end


