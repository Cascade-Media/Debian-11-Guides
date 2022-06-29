# Debian 11 Web Server
Quick setup & installation

> **Note**
> This script does not contain UFW

> **Warning**
> This script is currently untested.  


```.sh
sudo apt-get install unattended-upgrades apt-listchanges

sudo apt-get install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh

sudo nano /etc/ssh/ssh_config

    # Update From - To
    # PermitRootLogin prohibit-password
    # PermitRootLogin no

sudo apt install certbot python3-certbot-apache

sudo apt update && sudo apt upgrade -y
sudo apt-get install ca-certificates apt-transport-https software-properties-common wget curl lsb-release -y

curl -sSL https://packages.sury.org/php/README.txt | sudo bash -x
sudo apt update
sudo apt upgrade

sudo apt install php8.1 libapache2-mod-php8.1
sudo systemctl restart apache2

sudo apt install php8.1-fpm libapache2-mod-fcgid

sudo systemctl restart apache2

sudo systemctl status apache2
sudo systemctl status php8.1-fpm

php –version
php -m

sudo apt install php-curl php-mysql

php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"

sudo mv composer.phar /usr/local/bin/composer

composer about

sudo nano /etc/php/8.1/fpm/php.ini
    # Update memory Limit From - To
    # memory_limit = 128M
    # memory_limit 256M

sudo systemctl restart php8.1-fpm

sudo a2enmod rewrite
sudo a2enmod ssl
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo systemctl restart apache2

sudo apt update && sudo apt upgrade -y
sudo apt-get install curl software-properties-common dirmngr ca-certificates apt-transport-https -y

sudo apt install curl -y
sudo apt update

curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash -s -- --mariadb-server-version=10.8
sudo apt update

sudo apt install mariadb-server mariadb-client

mariadb –version
systemctl status mariadb

sudo systemctl start mariadb

sudo mysql_secure_installation

    # •	Switch to unix_socket authentication [Y/n] : Y
    # •	Change the root password? [Y/n] : Y
    # •	Remove anonymous users? [Y/n] : Y
    # •	Disallow root login remotely? [Y/n] Y
    # •	Remove test database and access to it? [Y/n] : Y
    # •	Reload privilege tables now? [Y/n] : Y

sudo mariadb-upgrade

sudo apt-get install php-mbstring
sudo apt -y update
sudo apt -y install wget php php-cgi php-pear php-mbstring libapache2-mod-php php-common php-phpseclib php-mysql php8.1-mysql

$ cd ~
$ DATA="$(wget https://www.phpmyadmin.net/home_page/version.txt -q -O-)" 
URL="$(echo $DATA | cut -d ' ' -f 3)" 
VERSION="$(echo $DATA | cut -d ' ' -f 1)" 
wget https://files.phpmyadmin.net/phpMyAdmin/${VERSION}/phpMyAdmin-${VERSION}-all-languages.tar.gz

tar xvf phpMyAdmin-${VERSION}-all-languages.tar.gz

sudo mv phpMyAdmin-*/ /var/www/html/services
cd /var/www/html/services/
sudo mv phpMyAdmin-5.2.0-all-languages phpmyadmin

cd /var/www/html/services/phpmyadmin
sudo mkdir temp

sudo chown -R www-data:www-data /var/www/html/services/phpmyadmin
sudo chmod 755 /var/www/html/services/phpmyadmin/temp/
sudo chown www-data:www-data /var/www/html/services/phpmyadmin/temp/

sudo cp /var/www/html/services/phpmyadmin/config.sample.inc.php  /var/www/html/services/phpmyadmin/config.inc.php

sudo nano /var/www/html/services/phpmyadmin/config.inc.php

    # Modify
    # /**
    # * This is needed for cookie based authentication to encrypt password in
    # * cookie. Needs to be 32 chars long.
    # */
    # $cfg['blowfish_secret'] = 'YOUR SECRET PASSPHRASE'; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */

    # Add
    # /** 
    # * Temporary Directory
    # */
    # $cfg['TempDir'] = '/var/www/html/services/phpmyadmin/temp';
```
