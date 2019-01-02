httpd
=====

There are many different HTTP servers out there, mainly `Apache </debian/apache-php.html>`_ and Nginx.

Each has its own security recommendations, which you'll find on their respective pages,
but there is a set of security tools that can be used on both.

sslyze
------

https://github.com/nabla-c0d3/sslyze

Get recommendations on your server's TLS implementation.

In the example below, 192.168.1.11 is my local apache.

::

    $ pip3 install sslyze
    $ python3 -m sslyze --regular 192.168.1.11

nikto
-----

https://cirt.net/nikto2

Scan your web server for known vulnerabilities.

::

    $ git clone https://github.com/sullo/nikto
    $ ./nikto/program/nikto.pl -host https://192.168.1.11
