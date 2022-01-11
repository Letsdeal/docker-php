FROM php:8.0-apache

RUN apt-get update && apt-get install -y \
        acl \
        git \
        zlib1g-dev \
        libicu-dev \
        libssl-dev \
        libzip-dev \
        libonig-dev \
        wget \
        unzip\
      && docker-php-ext-install zip intl mbstring pdo pdo_mysql opcache bcmath \
      && pecl install apcu mongodb xdebug \
      && docker-php-ext-enable apcu mongodb

ENV DOCKERIZE_VERSION v0.6.1
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

ENV COMPOSER_BIN_DIR /usr/local/bin
ENV COMPOSER_ALLOW_SUPERUSER 1
