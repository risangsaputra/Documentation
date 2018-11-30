
## Install Nginx Web Server And PHP New Version

###### Step 1. update & upgrade

```
sudo apt-get update -y && sudo apt-get upgrade -y 
sudo apt install nginx
```

###### Step 2. install the php-fpm and php-mysql packages:

```
sudo apt install php-fpm php-mysql
```

###### Step 3. Open the main php-fpm configuration file with root privileges:

```
sudo nano /etc/php/7.0/fpm/php.ini
```
We will change both of these conditions by uncommenting the line and setting it to "0" like this:

```
# /etc/php/7.0/fpm/php.ini

cgi.fix_pathinfo=0
```
Save and close the file when you are finished.

###### Step 4. Now, we just need to restart our PHP processor by typing:
```
sudo systemctl restart php7.0-fpm
```

###### Step 5. Configure Nginx to Use the PHP Processor
```
sudo nano /etc/nginx/sites-available/default
```
Before :
```
# /etc/nginx/sites-available/default

server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

After :
```
# /etc/nginx/sites-available/default

server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name server_domain_or_IP;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
When you've made the above changes, you can save and close the file.
###### Step 6. Test your configuration file for syntax errors by typing:

```
sudo nginx -t
```
###### Step 7. When you are ready, reload Nginx to make the necessary changes:
```
sudo systemctl reload nginx
```
###### Step 8. Create a PHP File to Test Configuration
>sudo nano /var/www/html/info.php

```
<?php
phpinfo();
?>
```

