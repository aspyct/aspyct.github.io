.. title:: Connecting to a Cisco console port via serial

Connecting to console
=====================

To connect to a Cisco console, you need the console cable.
If you computer doesn't have a serial port, you will also need
a USB to Serial converter.

Under debian, ``minicom`` can be used to connect.

::

    $ sudo apt-get install minicom
    $ sudo minicom -s

    > select "Serial port setup"
    > Serial device: /dev/ttyUSB0
    > Bps/Par/Bits: 9600 8N1
    > save as dfl

    $ sudo usermod -aG dialout <your user>

    > now log out and in again

    $ minicom
    > Hit enter once, the "Switch>" prompt will pop up.



