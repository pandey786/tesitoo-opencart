# tesitoo-opencart

How to set up server & corresponding database from this repo from scratch
=========================================================================

Environment
-----------

- apache 2.4
  - `sudo apt-get install apache2`
- mod_rewrite, mod_ssl, mod_headers enabled
  - `sudo apt-get install php5_mod_rewrite`
  - `sudo a2enmod rewrite`
  - `sudo a2enmod ssl`
  - `sudo a2enmod headers`
  - `sudo service apache2 restart`
- apache config:
  - for `/var/www` (or wherever content directory is), allow override
  
    - suggest per directory in `/etc/apache2/sites-available/000-default.conf`) :

            <Directory /var/www/html>
                Options FollowSymLinks
                AllowOverride All
            </Directory>

- php 5.x (5.6 recommended)
  - `sudo apt-get install php5 libapache2-mod-php5`
- mysqli (php extension)
- mysql 5.6
  - `sudo apt-get install mysql-server php5-mysql`
- phpMyAdmin (recommended)
  - `sudo apt-get install phpmyadmin`
  - `sudo ln -s /usr/share/phpmyadmin/ /var/www/html/phpmyadmin`

Procedure
-----------------

Using phpMyAdmin
- create a new database (suggest utf8mb4_general_ci)
- create a database user (for API to access)
- give new user all permissions except grant to new db

Opencart source
- clone this repository (https://github.com/projectwife/tesitoo-opencart)
  - `sudo apt-get install git`
  - `git clone https://github.com/projectwife/tesitoo-opencart.git`
- install in desired location within web-server content directory
- `cp config-dist.php config.php`
- `cp admin/config-dist.php admin/config.php`
- `cp api/v1/config-dist.php api/v1/config.php`
- edit `config.php`, `admin/config.php` and `api/v1/config.php` as necessary :

  - all the paths should be changed to correspond to your opencart location

	`DB_USERNAME`, `DB_PASSWORD` and `DB_DATABASE` should be set according to the database & user you created above

File permissions - make the following directories recursively writable by www-data
(apache default user)
- `vqmod/`
- `system/storage/logs/`
- `system/storage/cache/`
- `system/storage/download/`
- `system/storage/upload/`
- `system/storage/modification/`
- `image/cache/`
- `image/catalog/`

!!!FIXME need to find way to handle permissions / users programmatically!!!

Database set-up (phpMyAdmin)
- import mtesitoo.sql


SSL (Let's Encrypt)
---------------------
To install certbot
- `sudo apt-get install python-certbot-apache -t jessie-backports`
- `sudo certbot --apache`

To set up certificate renewal:
- `sudo crontab -e`

and add the line:
- `56 06,18 * * *   certbot renew`
