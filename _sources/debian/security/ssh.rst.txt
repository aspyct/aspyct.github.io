sshd
====

Login with your ssh key
-----------------------

Copy your public key to the server to allow login with private key.

.. code-block:: text

    $ ssh-copy-id <user>@<server>


/etc/ssh/sshd_config
--------------------

.. DANGER::
    Any time you modify `sshd_config(5) <https://manpages.debian.org/stretch/openssh-server/sshd_config.5.en.html>`_,
    make sure to test the changes **before** you close the connection to your server.
    You should reload the config with ``systemctl reload sshd``
    and open a new SSH connection before doing anything else.

Disallow root login and force protocol version 2.

::

    PermitRootLogin no
    Protocol 2

If you have copied your private key to the server, you can also disable password login:

::

    PasswordAuthentication no

You may also chose to restrict SSH access to certain users.

::

    AllowUsers aspyct

If your server is internet-facing, it could also be useful to change the
SSH port. It won't save you if your server is really targeted, but most
bot scans will skip the port. Just pick any random port above between
1025 and 65535.

.. DANGER::
    Don't forget to allow that port in the `firewall </debian/security/ufw.html>`_,
    otherwise you'll be locked out of your server for good.

::

    Port 12345



ssh-audit
---------

Run `ssh-audit <https://github.com/arthepsy/ssh-audit>`__ to check for
other vulnerabilities and recommendations. Keep at it until it's all
green.

Adding the following to ``sshd_config`` should be a good starting point:

::

    KexAlgorithms curve25519-sha256@libssh.org,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512,diffie-hellman-group14-sha256
    HostKeyAlgorithms rsa-sha2-512,rsa-sha2-256,ssh-rsa,ssh-ed25519
    MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com


External links
--------------

- `Official documentation <https://www.debian.org/doc/manuals/securing-debian-howto/ch-sec-services.en.html#s5.1>`_
- `sshd_config(5)`_
