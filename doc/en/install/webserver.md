## Webserver Configuration

Choose a server name for your project, and edit your
/etc/hosts file accordingly:

    127.0.0.2    nx

Note that 127.0.0.2 can be changed to any 127.\*.\*.\* address.

### nginx

Place this code block within the `http {}` block
in your nginx.conf file.

    server {
        server_name     nx;
        access_log      /var/log/nginx/nx/access.log;
        error_log       /var/log/nginx/nx/error.log;

        location / {
            root    /srv/http/nx/app/public;
            index   index.html index.htm index.php;
        }

        rewrite ^/js/(.+)\.js$ /js/$1.js break;
        rewrite ^/css/(.+)\.css$ /css/$1.css break;
        rewrite ^/img/(.+)\.(jpg|jpeg|png|gif)$ /img/$1.$2 break;
        rewrite ^/(.+)$ /index.php?url=$1 break;

        location ~ \.php$ {
            root            /srv/http/nx/app/public;       
            fastcgi_pass    unix:/var/run/php-fpm/php-fpm.sock;
            fastcgi_index   index.php;
            fastcgi_param   SCRIPT_FILENAME  
                            /srv/http/nx/app/public/$fastcgi_script_name;
            include         fastcgi_params;
        }
    }

Note that you will have to change the `server_name` to
the name you used above in your hosts file.

You will also have to adjust the directories according to where 
you checked out the code.  In this configuration, /srv/http/nx/ 
is the project root.  The public-facing part of your application, 
on the other hand, should always be app/public **within** the 
project root (so in this example, it's /srv/http/nx/app/public).

What's happening here, exactly?  The first three rewrite rules are 
responsible for routing any js, css, or img (jpg, jpeg, png, gif) 
file requests to their respective directories within the framework.
The last rewrite rule catches any other file requests and forwards it
to index.php (located within app/public).  Note that the 
/js, /css, and /img directories also exist within app/public - 
just be sure to remove the 'empty' files within them.

Restart your webserver, and then point your browser at the server name 
you chose above.  If you see a welcome screen, then you've configured 
everything correctly!

### lighttpd

    TODO

### apache

    TODO

