# Setup-Laravel-Ubunut-Apache2
How to setup Laravel on Ubuntu using Apache2 engine.

## The basics
>sudo apt update
>sudo apt install apache2

sudo systemctl stop apache2.service
sudo systemctl start apache2.service
sudo systemctl enable apache2.service
sudo apt install php7.2 libapache2-mod-php7.2 php7.2-mbstring php7.2-xmlrpc php7.2-soap php7.2-gd php7.2-xml php7.2-cli php7.2-zip
sudo nano /etc/php/7.2/apache2/php.ini
  memory_limit = 256M
  upload_max_filesize = 64M
  cgi.fix_pathinfo=0
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
cd /var/www/html
git clone https://github.com/username/project.git
sudo chown -R www-data:www-data /var/www/html/project/
sudo chmod -R 755 /var/www/html/project/
sudo nano /etc/apache2/sites-available/laravel.conf

<VirtualHost *:80>   
  ServerAdmin admin@example.com
     DocumentRoot /var/www/html/MyProject/public
     ServerName example.com

     <Directory /var/www/html/MyProject/public>
        Options +FollowSymlinks
        AllowOverride All
        Require all granted
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

sudo a2dissite 000-default.conf
sudo a2ensite laravel.conf
sudo a2enmod rewrite
sudo service apache2 restart

sudo systemctl restart apache2.service
