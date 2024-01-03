.. title:: Cisco Catalyst commands modes

Command modes
=============

Normal mode ``Switch>``
    Read-only access to some of the switch info.

    - ``enable`` to go into priviledged mode.

Priviledged mode ``Switch#``
    User is allowed to configure the switch.

    - ``disable`` to go back to normal mode.
    - ``config terminal`` to go to configuration mode.

Configuration mode ``Switch(config)#``
    General configuration of the switch.

    - ``line ...`` to go to line configuration mode.
    - ``interface ...`` to go to interface configuration mode.

Line conf mode ``Switch(config-line)#``
    Configuration of the command lines like the console port CON or the VTYs.

Interface conf mode ``Switch(config-if)#``
    Configure a specific interface. It can be a lot of things, including:

    - a port, ``fa1/0/1``
    - a vlan, ``vlan 1``
    - etc.
