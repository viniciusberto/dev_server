FROM ubuntu:18.04

#Add PHP repository to apt-list
RUN apt update
RUN apt install -y software-properties-common
RUN add-apt-repository ppa:ondrej/php -y
RUN apt update
#Install PHP and Extensions
RUN DEBIAN_FRONTEND="noninteractive" apt install -y php7.4 php7.4-cli php7.4-common php7.4-json php7.4-opcache php7.4-mysql php7.4-soap php7.4-mbstring php7.4-mcrypt php7.4-zip php7.4-fpm php7.0-mbstring php7.4-xml
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

#Install apache
RUN apt install -y apache2
RUN rm /var/www/html/*
#Install apache php extension
RUN apt install -y libapache2-mod-php7.4
#Enable apache rewrite module
RUN a2enmod rewrite
RUN cat /etc/apache2/apache2.conf | sed -e "s/AllowOverride none/AllowOverride All/" > /etc/apache2/apache2.conf.temp
RUN mv /etc/apache2/apache2.conf.temp /etc/apache2/apache2.conf
RUN cat /etc/apache2/apache2.conf | sed -e "s/AllowOverride None/AllowOverride All/" > /etc/apache2/apache2.conf.temp
RUN mv /etc/apache2/apache2.conf.temp /etc/apache2/apache2.conf

#Install Mysql Client
RUN apt update
RUN apt upgrade -y
RUN apt install -y mysql-client

WORKDIR /var/www/html

EXPOSE 80
EXPOSE 3306

CMD apachectl -D FOREGROUND
