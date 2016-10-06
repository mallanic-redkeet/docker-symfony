FROM alpine:edge

MAINTAINER Vincent Composieux <vincent.composieux@gmail.com>

RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories
RUN apk add --update \
    php7 \
    php7-fpm \
    php7-apcu \
    php7-curl \
    php7-common \
    php7-cgi \
    php7-gmagick \
    php7-gd \
    php7-iconv \
    php7-json \
    php7-intl \
    php7-mcrypt \
    php7-opcache \
    php7-openssl \
    php7-pdo \
    php7-pdo_mysql \
    php7-phar \
    php7-posix \
    php7-session \
    php7-xml \
    php7-dom \
    php7-mysqli \
    php7-ctype \
    php7-xml \
    php7-zlib \
    nginx

RUN adduser -D -g '' -G www-data www-data

# PHP

RUN rm -rf /etc/php7/conf.d/10_.ini

RUN ln -s /usr/bin/php7 /usr/bin/php

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === 'e115a8dc7871f15d853148a7fbac7da27d6c0030b848d9b3dc09e2a0388afed865e6a3d6b3c0fad45c48e2b5fc1196ae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"

RUN rm -rf /var/cache/apk/* && rm -rf /tmp/*

ADD php-fpm/symfony.ini /etc/php7/conf.d/symfony.ini
ADD php-fpm/symfony.pool.conf /etc/php7/php-fpm.d/www.conf

# Nginx
RUN rm -rf /etc/nginx/conf.d/*
ADD nginx/nginx.conf /etc/nginx/
ADD nginx/symfony.conf /etc/nginx/conf.d/
RUN chmod 755 -R /var/lib/nginx/tmp

WORKDIR /var/www/symfony

CMD chmod -R 777 /var/www/symfony/var && php-fpm7 -F & nginx

EXPOSE 80
