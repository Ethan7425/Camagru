FROM php:8.4-fpm-alpine

# System libraries needed to compile the PHP extensions
RUN apk add --no-cache \
        postgresql-dev \
        libpng-dev \
        libjpeg-turbo-dev \
        freetype-dev \
        msmtp

# gd: image merging (PNG + JPEG + fonts)
# pdo_pgsql: talk to PostgreSQL
RUN docker-php-ext-configure gd --with-jpeg --with-freetype \
    && docker-php-ext-install -j"$(nproc)" gd pdo_pgsql

# Our PHP and PHP-FPM settings ("zz-" so it loads after the defaults)
COPY php.ini /usr/local/etc/php/conf.d/camagru.ini
COPY fpm.conf /usr/local/etc/php-fpm.d/zz-camagru.conf

# Folder for the PHP error log, writable by the PHP worker user
RUN mkdir -p /var/log/php && chown www-data:www-data /var/log/php

WORKDIR /var/www
