FROM php:7.0.8-fpm

MAINTAINER "ty42 <imty42@gmail.com>"

ADD pecl-memcache.tar.gz /usr/src/php/ext/

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libmcrypt4 \
        libmcrypt-dev \
        libpng12-dev \
        libjpeg-dev \
        libmagickwand-dev \
        --no-install-recommends
RUN docker-php-ext-install -j$(nproc) pecl-memcache \
    && docker-php-ext-install iconv mcrypt opcache \
    && docker-php-ext-configure gd --enable-gd-native-ttf --with-freetype-dir=/usr/include/freetype2 --with-png-dir=/usr/include --with-jpeg-dir=/usr/include \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install gettext \
    && pecl install imagick \
    && docker-php-ext-enable imagick
