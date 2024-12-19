OpenBSD
=======

Disk label conventions
----------------------

-  a: root of the system (on a boot disk)
-  b: swap of the system (on a boot disk)
-  c: whole OBSD partition
-  i: points to the first MBR partition on the disk

Formatting a USB drive to be a keydisk
--------------------------------------

Instructions assume usb drive has been assigned sd1 during boot. See
dmesg. Also, you might have to create the node:

.. code-block:: text

    cd /dev && sh MAKEDEV sd1

First, create the MBR:

.. code-block:: text

    # fdisk -iy sd1
    # disklabel sd1

Then, create partitions as needed on the drive. I suggest making a big
MSDOS partition to still be able to use the key (beware untrusted
machines), and an OpenBSD partition at the end to store the keys.

In our example, the disk has 15.133.248 sectors of 512 bytes available.
That's roughly 8Gb. We'll devote 11Mo at the end of the disk to the
keys. We'll need around 22.000 sectors to do that.

The MBR will already contain 1 big OpenBSD partition spanning the whole
disk. We need to resize it.

.. code-block:: text

    # fdisk -e sd1
    > edit 3

    -- move partition to offset 15133248 - 22000 = 15111248
    -- use all remaining space, i.e. 22000 sectors
    -- Also, create the MSDOS partition at the beginning
    -- Feel free to leave unused space for further things

    > edit 1
    -- Set size to 4g

    -- Write and quit
    > w
    > q

Now we need to create the labels on the OBSD partition.

.. code-block:: text

    # disklabel -E sd1

    -- Get some info on the drive
    -- At this point, labels c and i should already exist
    > p

    -- Create the label a for the keydisk
    > a a
    offset: default
    size: 2000
    FS type: RAID

    > w
    > q

Reset a FDE disk
----------------

If you're trying to setup a disk with bioctl that was already encrypted,
the command will fail. You need to detach the disk first.

That may require a ``sh MAKEDEV`` command as well.

Careful, this will possibly delete other data on your disk.

.. code-block:: text

    # bioctl -d sd3
    # dd if=/dev/zero of=/dev/rsc0c bs=10m count=1

Format a USB HDD for use with dump(8)
-------------------------------------

1. Plug the disk in, and find out what /dev/sd\* device it's attached to
   with ``dmesg``. You should see something like this:

.. code-block:: text

    umass0 at uhub0 port 5 configuration 1 interface 0 "ASMedia AS2115" rev 3.00/0.01 addr 2
    umass0: using SCSI over Bulk-Only
    scsibus2 at umass0: 2 targets, initiator 0
    sd1 at scsibus2 targ 1 lun 0: <ASMT, 2115, 0> SCSI4 0/direct fixed serial.174c1153000000000000
    sd1: 476940MB, 512 bytes/sector, 976773168 sectors

From here on, the disk will be referred to as ``sd1``. Adjust the
commands according to your setup.

2. Prepare an MBR sector on the disk

.. code-block:: text

    # fdisk -iy sd1

If you still have the default MBR template, your disk will be one big
OpenBSD partition. Check this with ``fdisk``

.. code-block:: text

    # fdisk sd1
    Disk: sd1       geometry: 60801/255/63 [976773168 Sectors]
    Offset: 0       Signature: 0xAA55
                Starting         Ending         LBA Info:
     #: id      C   H   S -      C   H   S [       start:        size ]
    -------------------------------------------------------------------------------
     0: 00      0   0   0 -      0   0   0 [           0:           0 ] unused
     1: 00      0   0   0 -      0   0   0 [           0:           0 ] unused
     2: 00      0   0   0 -      0   0   0 [           0:           0 ] unused
    *3: A6      0   1   2 -  60800 254  63 [          64:   976768001 ] OpenBSD

Then follow the instructions at
https://www.openbsd.org/faq/faq14.html#softraidCrypto to encrypt the
disk as needed.

Mount an encrypted backup disk
------------------------------

Create the mount directory if it doesn't exist yet.

.. code-block:: text

    # mkdir -p /mnt/encrypted_removable

Mount the drive. Note the distinction between the physical drive, and
the encrypted drive.

.. code-block:: text

    # bioctl -c C -l sd1a softraid0
    > passphrase

Now the software encrypted disk should be ready to be mounted.

.. code:: text

    # mount /dev/sd3i /mnt/encrypted_removable

Troubleshooting
---------------

mount: invalid argument
~~~~~~~~~~~~~~~~~~~~~~~

If you just created the partition and the label, did you also run
``newfs``? See https://www.openbsd.org/faq/faq14.html#softraidCrypto
