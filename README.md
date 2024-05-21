
### CONNECT
ssh -i pemfile user@ipv4public

### ROOT
sudo su

### UPDATE DENPENCY
apt-get update

### INSTALL NGINX
add-apt-repository ppa:nginx/stable

apt-get update

apt-get install nginx

### INSTALL PHP-FPM
add-apt-repository ppa:ondrej/php

apt-get update

apt-get install php php-fpm php-xml php-gd php-opcache php-mbstring

### INSTALL MYSQL AND SET USER
apt-get install mysql-server

mysql -u root -p

CREATE DATABASE database_name;

CREATE USER 'your_username'@'localhost' IDENTIFIED BY 'your_password';

GRANT ALL PRIVILEGES ON database_name . * TO 'your_username'@'localhost';

FLUSH PRIVILEGES;

EXIT;

### INSTALL COMPOSER
cd /var/www

wget https://getcomposer.org/download/latest-stable/composer.phar

### CHECK FREE MEMORY / ADD SWAP
free -m

/bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024

/sbin/mkswap /var/swap.1

/sbin/swapon /var/swap.1


### INSTALL LARAVEL FROM GIT
cd /var/www

git clone -b your_branch your_repo_url

cd /your_repo_name

cp .env.example .env

php ../composer.phar install

php artisan key:generate

php artisan migrate

php artisan make:auth

### PERMISSION FOLDER LARAVEL
chown -R www-data:www-data storage/

### NGINX CONFIG FOR LARAVEL
vi /etc/nginx/sites-available/default

```
server {
    listen 80;
    server_name _;
    root your_repo_path;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php{php_version}-fpm.sock; #Replace {php_version} with the number of php version you installed
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

### IMPORT DATA FROM SQL FILE INTO MYSQL VIA COMMAND LINE
mysql -u your_username -p your_password -D your_database < your_sql_file.sql

### NGINX CHECK STATUS CONFIG
nginx -t


### MIGRATING FROM APACHE TO NGINX ON AN UBUNTU VPS
sudo service apache2 stop

sudo systemctl disable apache2


### NGINX RESTART SERVICE AND CHECK STATUS
service nginx restart

service nginx status
