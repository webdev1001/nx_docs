## PHP Configuration

You'll want to make the following adjustments to your php.ini file.
Not only will they enhance the security of your web application,
they will also encourage you to code with best practices in mind.

    cgi.fix_pathinfo=0
    short_open_tag = Off
    error_reporting = E_ALL | E_STRICT

NX requires that the following extensions be enabled:

    extension=curl.so
    extension=json.so
    ; Note that you will most likely have to build
    ; the memcached extension yourself. See
    ; http://www.php.net/manual/en/memcached.installation.php
    ; for more information.
    extension=memcached.so

The extension below is optional.  Enable it only if you plan
on using MySQL.

    extension=pdo_mysql.so
