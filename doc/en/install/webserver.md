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
	     root            /srv/http/nx/app/public;
	     index           index.php index.html;

	     access_log      /var/log/nginx/nx/access.log;
	     error_log       /var/log/nginx/nx/error.log;

	     location / {
            try_files $uri /index.php?url=$uri;
	     }

	     location ~ \.php$ {
            fastcgi_pass    unix:/var/run/php-fpm/php-fpm.sock;
            fastcgi_index   index.php;
            fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include         fastcgi_params;
	     }
	 }


Note that you will have to change the `server_name` to
the name you used above in your hosts file.  You will also 
have to adjust the directories according to where you checked 
out the code.  In this configuration, /srv/http/nx/ is the 
project root.  The public-facing part of your application, 
on the other hand, should always be app/public **within** the 
project root (so in this example, it's /srv/http/nx/app/public).

What's happening here, exactly?  The try\_files directive will check
to see if the resouce at $uri exists in the filesystem (in this
example, within /srv/http/nx/app/public).  If it does, that file is
served by nginx.  If it doesn't, it's then routed to /index.php,
whereupon the framework takes responsibility for parsing the url
and handling the request.  The try\_files directive is great for 
serving static content - there's no need to pass requests for
js, css, or image files through the framework.

Restart your webserver, and then point your browser at the server name 
you chose above.  If you see a welcome screen, then you've configured 
everything correctly!

### lighttpd

    TODO

### apache

    TODO

