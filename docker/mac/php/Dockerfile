FROM php:8-fpm-alpine

ENV COMPOSER_HOME=/root/composer \
    PATH=$COMPOSER_HOME/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1 \
    DEBCONF_NOWARNINGS=yes \
    DOCKERIZE_VERSION=v0.6.1

# package install and setup
RUN set -eux \
 && apk add --update-cache --no-cache openssl git autoconf g++ nodejs-current npm yarn postgresql-dev libtool make libzip-dev libpng-dev libjpeg-turbo-dev freetype-dev \
 && pecl install redis \
 && docker-php-ext-configure gd --with-jpeg=/usr \
 && docker-php-ext-configure opcache --enable-opcache \
 && docker-php-ext-install gd pdo_pgsql pgsql opcache bcmath gd exif zip \
 && docker-php-ext-enable redis \
 && apk del autoconf g++ libtool make && rm -rf /tmp/* \
 && wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
 && tar -C /usr/local/bin -xzvf dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
 && rm dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
 && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# project
ARG PROJECT_DOMAIN
COPY . /opt/$PROJECT_DOMAIN
