FROM php:7.1-apache

RUN apt-get update -y && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng12-dev

RUN docker-php-ext-configure \
    gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) \
        gd \
        pdo \
        pdo_mysql

RUN pecl install xdebug-2.5.4 \
    && docker-php-ext-enable xdebug

COPY config/php.ini /usr/local/etc/php/

RUN a2enmod rewrite
RUN service apache2 restart