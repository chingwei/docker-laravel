FROM ubuntu:18.04
MAINTAINER chingwei

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update --fix-missing
RUN apt-get install -y nginx
RUN apt-get install -y php7.2 php7.2-mbstring php7.2-xml php7.2-curl php7.2-fpm php7.2-zip php7.2-imagick
RUN apt-get install -y php7.2-gd   # for excel PhpSpreadsheet's install
RUN apt-get install -y php7.2-sybase # for connect to mssql
RUN apt-get install -y php-xdebug
RUN apt-get install -y php7.2-pgsql
RUN apt-get install -y vim zip
RUN apt-get install -y cron
RUN mkdir -p /run/php/
RUN mkdir -p /home/www/

# Install Laravel + Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === '93b54496392c062774670ac18b134c3b3a95e5a5e5c8f1a9f115f203b75bf9a129d5daa8ba6a13e2cc8a1da0806388a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv /composer.phar /usr/bin/composer


COPY laravel.conf /etc/nginx/sites-available/
RUN ln -s /etc/nginx/sites-available/laravel.conf /etc/nginx/sites-enabled/laravel.conf
RUN rm /etc/nginx/sites-enabled/default

# Add Crontab
COPY crontab /crontab
#RUN cat crontab | crontab -  # root's cron
RUN cat crontab | crontab -u www-data -
#RUN service cron start
#RUN update-rc.d cron start 1 2 3 4 5 6 

# Fix timezone
RUN cp /usr/share/zoneinfo/Asia/Taipei /etc/localtime

COPY start_nginx.sh /start_nginx.sh
COPY start_php-fpm.sh /start_php-fpm.sh
COPY start_cron.sh /start_cron.sh
COPY wrapper.sh /wrapper.sh
RUN chmod +x /start_nginx.sh /start_php-fpm.sh /start_cron.sh /wrapper.sh
CMD ["/wrapper.sh"]

#CMD ["nginx", "-g", "daemon off;"]
