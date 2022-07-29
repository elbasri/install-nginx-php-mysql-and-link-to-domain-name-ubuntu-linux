# تثبيت وإنشاء موقع ووردبريس على مخدم VPS
# تثبيت انجينكس بي اتش بي مايسكيول في نظام اوبنتو 20.04 لينكس
# install wordpress nginx php mysql and link to domain name ubuntu linux

sudo apt update

sudo apt install nginx

systemctl start nginx

systemctl enable nginx

systemctl status nginx

sudo apt install mysql-server mysql-client

systemctl start mysql

systemctl enable mysql

systemctl status mysql

mysql_secure_installation

sudo apt install php-fpm php-cli php-curl php-mysql php-curl php-gd php-mbstring php-pear -y

cd /etc/php/7.4/fpm/
nano php.ini

cgi.fix_pathinfo = 0

systemctl start php7.4-fpm

systemctl enable php7.4-fpm

systemctl status php7.4-fpm

ss -pl | grep php

cd /etc/nginx/sites-available/


server {

    server_name wordpress.nacer.ma;

    root   /var/www/html/nacer;
    index  index.php index.html;

    access_log /var/log/nginx/nacer_access.log;
    error_log /var/log/nginx/nacer_error.log;

    location / {
        try_files $uri $uri/ /index.php?url=$request_uri;
    }

    location ~ \.php$ {
        fastcgi_pass   unix:/run/php/php7.4-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
        fastcgi_intercept_errors on;
    }
}

