FROM php:8.2-apache
ARG XDEBUG_ENABLED=0
ARG MEMINFO_ENABLED=0
ARG USERNAME=app-user
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the non-root user
# See https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Add www-data to non-root user's group
RUN usermod --gid $USER_GID www-data

#RUN curl -sL https://deb.nodesource.com/setup_16.x | sudo bash -
RUN apt-get update -y && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libwebp-dev \
    libavif-dev \
    libicu-dev \
    libzip-dev \
    libssl-dev \
    libc-client-dev \
    libkrb5-dev \
    zip \
    subversion \
    git \
    ant \
    nodejs \
    npm

RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ --with-webp=/usr/include/ --with-avif=/usr/include/ \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install -j$(nproc) \
        gd \
        pdo \
        pdo_mysql \
        mysqli \
        intl \
        exif \
        zip \
        imap \
        opcache

RUN if [ "$XDEBUG_ENABLED" = "1" ]; then pecl install xdebug \
    && docker-php-ext-enable xdebug \
    ; fi

RUN if [ "$MEMINFO_ENABLED" = "1" ]; then curl -fsSL 'https://github.com/BitOne/php-meminfo/archive/master.tar.gz' -o meminfo.tar.gz \
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
	&& docker-php-ext-enable meminfo \
  ; fi

COPY config/php.ini /usr/local/etc/php/

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

COPY config/vhosts.conf /etc/apache2/sites-available/vhosts.conf

RUN a2enmod rewrite
RUN a2ensite vhosts.conf

# Run as non-root user
USER $USERNAME
ENV HOME /home/$USERNAME