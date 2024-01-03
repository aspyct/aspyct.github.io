Nitrokey Storage 2
==================

Verify and install firmware update
----------------------------------

1. Get the latest release, shasum and sig files from
   https://github.com/Nitrokey/nitrokey-storage-firmware/releases
2. Verify the sha sum

::

    $ sha256sum -c storage-firmware-V0.53-0-g905976e.hex.sha256sum
    storage-firmware-V0.53-0-g905976e.hex: OK

3. Verify the signature

::

    $ gpg --verify storage-firmware-V0.53-0-g905976e.hex.sig
    gpg: assuming signed data in 'storage-firmware-V0.53-0-g905976e.hex'
    gpg: Signature made Fri 23 Nov 2018 12:03:06 PM CET
    gpg:                using RSA key 868184069239FF65DE0BCD7DD9BAE35991DE5B22

Also manually check the RSA key fingerprint.

4. Update the firmware

::

    $ sudo dfu-programmer at32uc3a3256s erase
    $ sudo dfu-programmer at32uc3a3256s flash --suppress-bootloader-mem storage-firmware-V0.53-0-g905976e.hex
    $ sudo dfu-programmer at32uc3a3256s start
