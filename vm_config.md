This is the config I use for my VMs and/or prod server.
A lot of things will not be useful for you.

I'm using Debian 7 like OS : https://www.debian.org/

## MySQL, PHP, NginX installation

```
// First, update the packages db
$> sudo apt-get update

// Installation of MySQL
$> sudo apt-get install mysql-server mysql-client

// Installation of the server
$> sudo apt-get install nginx

// Installation of PHP on NginX
$> sudo apt-get install php5-fpm php5-mysql

// If not already do, edit `/etc/php5/fpm/pool.d/www.conf`
// change
// listen = 127.0.0.1:9000
// to
// listen = /var/run/php5-fpm.sock

// That's it !
```

## Vhost file

```
// Create vhost file in /etc/nginx/sites-available
$> sudo nano /etc/nginx/site-available/domain.local
server {
	listen 127.0.0.1:80;
	
	root /home/www/domain;
	index index.php index.html index.htm;
	
	server_name domain.local;
	
	location / {
		try_files $uri $uri/ /index.html;
	}
	
	location ~ \.php$ {
		try_files $uri =404;
		fastcgi_pass unix:/var/run/php5-fpm.sock;
		fastcgi_index index.php;
		fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
		include fastcgi_params;
	}
	
	location ~ /\.ht {
	deny all;
	}
}

// Create the symbolic link
$> ln -s /etc/nginx/site-available/domain.local /etc/nginx/site-enabled/domain.local
// Edit the hosts file
$> sudo nano /etc/hosts
127.0.0.0.1		domain.local
// restart the server
$> sudo service nginx restart
```

## Phalcon installation

I use PhalconPHP Framework (https://phalconphp.com/en/).

```
// Installation of Git
$> sudo apt-get install git

// Installation of tools required for compilation
$> sudo apt-get install php5-dev gcc libpcre3-dev build-essential
$> cd
$> git clone --depth=1 git://github.com/phalcon/cphalcon.git
$> cd cphalcon/build
$> sudo ./install

// Load the phalcon extension in 2 files
$> sudo nano /etc/php5/mods-available/phalcon.ini
$> sudo nano /etc/php5/fpm/php.ini
// In both files you must write :
extension=phalcon.so

// Finally you create a symbolic link in modules of php5-fpm
$> ln -s /etc/php5/mods-available/phalcon.ini /etc/php5/fpm/conf.d/50-phalcon.ini
// You can restart your server
$> sudo service php5-fpm restart
$> sudo service nginx restart
```

## MCrypt

Sometimes you want (de)crypt with phalcon, so you can easily install the library

```
$> sudo apt-get install php5-mcrypt
$> sudo php5enmode mcrypt
$> sudo service nginx restart
```

## Create a 'webmaster' user to access the sites

I like use an unique user to edit my files, without root rights. So this is how I create my user.
Generally my sites are in the home, you can adapt the commands for your usage.

```
$> sudo useradd webmaster -g www-data -s /bin/bash -d /home/www
$> sudo mkdir /home/www
$> sudo chgrp www-data /home/www
$> sudo chmod 775 /home/www
$> sudo passwd webmaster
```

NOTE : Sometimes, I don't know why, the user can't create/edit files/folders in his home, so I force by setting the ACL Rights :

```
$> sudo setfacl -R -m webmaster:rwx /home/www
```
