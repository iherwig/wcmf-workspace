FROM php:7.1-apache
ARG XDEBUG_ENABLED=0

RUN apt-get update -y && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng12-dev \
    libicu-dev

RUN docker-php-ext-configure \
    gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) \
        gd \
        pdo \
        pdo_mysql \
        mysqli \
        intl \
        exif \
        opcache

RUN if [ "$XDEBUG_ENABLED" = "1" ]; then pecl install xdebug-2.5.4 \
    && docker-php-ext-enable xdebug \
    ; fi

COPY config/php.ini /usr/local/etc/php/

RUN a2enmod rewrite
RUN service apache2 restart