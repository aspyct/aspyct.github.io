sudo
====

``sudo`` allows to run privileged commands without logging in as the root user,
thus avoiding the "root session left open" syndrome.

It also allows sharing administrative access to a machine without sharing the root password,
logging etc.

Quickstart
----------

1. Install the ``sudo`` package.

.. code-block:: text

    $ su -c "apt-get update"
    $ su -c "apt-get install sudo"

2. Add admin users to ``sudo`` group

.. code-block:: text

    $ su -c "usermod -aG sudo <user>

3. Logout, login again, and test

.. code-block:: text

    $ sudo whoami
