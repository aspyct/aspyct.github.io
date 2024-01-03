.. title:: Cisco Catalyst administrative access configuration

Administrative access
=====================

Set the hostname
----------------

::

    Switch(config)# hostname SW-2.4

Assign a management IP address
------------------------------

From user mode,

::

    Switch> enable
    Switch# conf t
    Switch(config)# int vlan1
    Switch(config-if)# ip address 192.168.1.51 255.255.255.0
    Switch(config-if)# no shutdown

You might want to assign a default gateway as well.

Set the default gateway
-----------------------

::

    Switch(config)# ip default-gateway 192.168.1.1
    Switch(config)# end

Set the console password
------------------------

::

    Switch(config)# line con 0
    Switch(config-line)# password cisco
    Switch(config-line)# login
    Switch(config-line)# exit
    Switch(config)# line vty 0 15
    Switch(config-line)# password cisco
    Switch(config-line)# login
    Switch(config-line)# end
    Switch# exit

The console, "normal mode" password is now set to "cisco". The ``login`` command enables the password prompt.

By default, this password will also apply to access the priviledged mode,
but you can set a priviledged-specific password with ``(config)#enable secret``.

Set the priviledged mode password
---------------------------------

::

    SW2(config)#enable secret mypassword


In addition to the console login password, you may set an administrator password
that will be required when entering the priviledged mode.

The secret is useful because it will not show as clear text in the ``show run | i enable``
command.

::

    SW2#show run | i enable
    enable secret 5 $1$Lnz4$CiqdpdGC45AFOXgcUJnbt/

5 is the level of hashing. Some switches support better hashing with PBKDF2 or SCrypt.

Set the inactivity timeout
--------------------------

::

    SW2(config)#line con 0
    SW2(config-line)#exec-timeout 2 30
    SW2(config-line)#exit
    SW2(config)#line vty 0 15
    SW2(config-line)#exec-timeout 2 30
    SW2(config-line)#exit

You'll be logged out after 2 minutes and 30 seconds from console or VTY.

In a lab, it's sometimes useful to disable timeout altogether.

::

    SW2(config)#line con 0
    SW2(config-line)#exec-timeout 0
    SW2(config-line)#exit

It's considered bad practice in a production environment for security purposes, though.

Enable password encryption
--------------------------

::

    SW2(config)#service password-encryption
    SW2#show run | i password
    service password-encryption
     password 7 060506324F41440A1245

.. WARNING::

    This will enable password mode "7", which is super weak
    and can be readily cracked on `some websites <http://www.ifm.net.nz/cookbooks/passwordcracker.html>`_.
    It will only prevent over-the-shoulder password theft.

Change the login banner
-----------------------

::

    SW2(config)#banner login ;
    Enter TEXT message.  End with the character ';'.
    Hey you're not supposed to be in here.
    ;

Configure Telnet
----------------

**Don't**.

But if you really must (like me, since my old cisco switches don't support SSH)...

::

    Switch(config)# line vty 0 15
    Switch(config-line)# transport input telnet
    Swicth(config-line)# end


Save the configuration
----------------------

By default, the running configuration will be lost when the switch reboots.
We need to store the running configuration into the startup config's NVRAM (non-volatile ram).

::

    SW2#copy running-config startup-config
    Destination filename [startup-config]?
    Building configuration...
    [OK]

.. NOTE::
    You can also simple run ``wr``, but this is deprecated.