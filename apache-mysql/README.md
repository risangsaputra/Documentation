# Install Apache2, Php5.6, Mysql 5.7 & Phpmyadmin


###### Step 1. Install software-properties-common

```
sudo apt-get install software-properties-common
```

###### Step 2. Add PPA 

```
sudo add-apt-repository ppa:ondrej/php
```

###### Step 3. Install Apache2 

```
sudo apt-get install apache2
```

###### Step 4. Add code 
>/etc/apache2/site-available/000-default.conf

before
```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
        #<Directory /var/www/html>
        #Options Indexes FollowSymLinks MultiViews
        #AllowOverride All
        #Require all granted
        #</Directory>

</VirtualHost>

```

after

```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn
        <Directory /var/www/html>
                Options +FollowSymLinks
                AllowOverride All
                Order allow,deny
                allow from all
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
        #<Directory /var/www/html>
        #Options Indexes FollowSymLinks MultiViews
        #AllowOverride All
        #Require all granted
        #</Directory>

</VirtualHost>

```

###### Step 4. Install PHP version

```
sudo apt-get update
sudo apt-get install php5.6
```

###### Step 5. You can install php5.6 modules

```
sudo apt-get install php5.6-mbstring php5.6-mcrypt php5.6-mysql php5.6-xml php5.6-curl
```
###### Step 6. Install mysql-server 5.7

```
sudo apt-get install mysql-server
```
###### Step 7. Install Phpmyadmin

```
sudo apt-get install -y phpmyadmin
```