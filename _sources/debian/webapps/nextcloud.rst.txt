NextCloud
=========

`NextCloud <https://nextcloud.com/>`_ is a "make your own cloud" solution.
It provides webdav, caldav, carddav and a lot of other features, depending on how you configure it.

Requirements
------------

NextCloud requires `apache and php </debian/apache-php.html>`_ to be installed.

It also depends on a bunch of php modules that can be installed with `apt-get(8) <https://manpages.debian.org/stretch/apt/apt-get.8.en.html>`_.
A database driver will be required. In this example, we chose sqlite3.

::

    $ sudo apt-get install php-xml php-zip php-curl php-gd php-mbstring php-sqlite3
    $ sudo apache2ctl restart

Quickstart
----------

At the time of writing, NextCloud provides a `web
installer <https://nextcloud.com/install/#instructions-server>`__, which
is probably the easiest way to install the app.
