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
    
RUN curl -fsSL 'https://github.com/BitOne/php-meminfo/archive/master.tar.gz' -o meminfo.tar.gz \
	&& mkdir -p meminfo \
	&& tar -xf meminfo.tar.gz -C meminfo --strip-components=1 \
	&& rm meminfo.tar.gz \
	&& ( \
		cd meminfo/extension/php7 \
		&& phpize \
		&& ./configure --enable-meminfo \
		&& make -j$(nproc) \
		&& make install \
	) \
	&& rm -r meminfo \
	&& docker-php-ext-enable meminfo

COPY config/php.ini /usr/local/etc/php/

RUN a2enmod rewrite
RUN service apache2 restart