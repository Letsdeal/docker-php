# Fix https://github.com/symfony/symfony/issues/32395
FROM php:7.3-apache

RUN apt-get update && apt-get install -y \
        acl \
        git \
        zlib1g-dev \
        libicu-dev \
        libssl-dev \
        libzip-dev \
        wget \
        unzip\
      && docker-php-ext-install zip intl mbstring pdo pdo_mysql opcache bcmath \
      && pecl install apcu mongodb xdebug \
      && docker-php-ext-enable apcu mongodb

ENV DOCKERIZE_VERSION v0.6.1
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer

ENV COMPOSER_BIN_DIR /usr/local/bin
ENV COMPOSER_ALLOW_SUPERUSER 1
