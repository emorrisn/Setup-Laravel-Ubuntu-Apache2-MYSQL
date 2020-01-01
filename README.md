# How to setup Laravel on Ubuntu using Apache2 and MYSQL as the database
How to setup Laravel on Ubuntu using Apache2 engine with a MYSQL database.

```
Ubuntu, Apache, Apache2, Laravel, 2019, 2020, OVH, MYSQL
```

## The basics  
```
sudo apt install apache2
sudo apt-get update
```

## Making sure Apache2 is working  
```
sudo systemctl stop apache2.service  
sudo systemctl start apache2.service  
sudo systemctl enable apache2.service  
```
## Install the PHP essentials  
```
sudo apt install php7.2 libapache2-mod-php7.2 php7.2-mbstring php7.2-xmlrpc php7.2-soap php7.2-gd php7.2-xml php7.2-cli php7.2-zip php7.2-mysql
```
or
```
sudo apt install php libapache2-mod-php php-mbstring php-xmlrpc php-soap php-gd php-xml php-cli php-zip php-mysql
```
## Note
```
Ensure you do "php -v" and replace "7.2" with wahtever version you recieve.
```

## Setup PHP's config  
```
sudo nano /etc/php/7.2/apache2/php.ini
```
### Edit the values 
```
memory_limit = 256M  
upload_max_filesize = 64M  
cgi.fix_pathinfo=0
```

## Get composer  
```
sudo apt install curl
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

## Getting your project  
```
cd /var/www/html  
git clone https://github.com/username/project.git
```
## Make sure all permissions are correct  
```
sudo chown -R www-data:www-data /var/www/html/project/  
sudo chmod -R 755 /var/www/html/project/  
```

## Sorting out the config  
```
sudo nano /etc/apache2/sites-available/laravel.conf
```
### Copy and paste
```
<VirtualHost *:80>
  ServerAdmin admin@example.com
     DocumentRoot /var/www/html/project/public
     ServerName project.io

     <Directory /var/www/html/project/public>
        Options +FollowSymlinks
        AllowOverride All
        Require all granted
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

## Enable the config
```
sudo a2dissite 000-default.conf
sudo a2ensite laravel.conf
sudo a2enmod rewrite
sudo service apache2 restart
```

## Make sure Apache2 is happy..
```
sudo systemctl restart apache2.service
```

## Setting up MYSQL
```
sudo ufw allow in "Apache Full"
sudo apt install mysql-server
sudo mysql_secure_installation
```
### Installation
```
Password Plugin: Y
Password Validation Policy: 1 (can be 2 if you want)
(Enter Password and save somewhere)
Continue with Password: Y
Remove Anonymous Users: Y
Disallow Root Login Remotely: N
Remote Test DB: Y
Reload Privileges: Y
```

## Creating the database
```
sudo mysql -u root -p
CREATE DATABASE db_name;
GRANT ALL ON db_name.* to 'db_user'@'localhost' IDENTIFIED BY 'db_password';
FLUSH PRIVILEGES;
quit
```

## Make your project is happy
```
cd /var/www/html/project
composer install
composer update
mv .env.example .env
(Enter Database Credentials)
php artisan config:cache
```

## Setting up your project
```
php artisan key:generate
php artiasn migrate:fresh --seed
```
