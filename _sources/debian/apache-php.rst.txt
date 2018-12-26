Apache & PHP
============

Quickstart
----------

::

    # Install necessary packages
    $ sudo apt-get install apache2 php libapache2-mod-php

    # Allow the port in your firewall
    $ sudo ufw allow http

    # Make sure apache is started
    $ sudo apachectl start

SSL with self-signed certificate
--------------------------------

::

    # Generate a self-signed certificate
    $ sudo apt-get install ssl-cert

    # Reconfigure apache
    $ sudo a2enmod ssl
    $ sudo a2ensite default-ssl

    # Allow the port in your firewall
    $ sudo ufw allow https

    # Reload apache configuration
    $ sudo apachectl reload apache2

Security
----------------

1. Review and edit ``/etc/apache2/conf-enabled/security.conf`` as needed
2. Install ``libapache2-modsecurity``


Documentation
-------------

Debian has its own configuration layout for apache.
You can read it directly from your SSH.

::

    $ sudo apt-get install apache2-doc
    $ gzip -d /usr/share/doc/apache2/README.Debian.gz -c | less
