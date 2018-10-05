FROM php:7.2-apache

RUN apt-get update && apt-get install -y \
        acl \
        git \
        zlib1g-dev \
        libicu-dev \
        libssl-dev \
        wget \
        unzip\
      && docker-php-ext-install zip intl mbstring pdo pdo_mysql opcache bcmath \
      && pecl install apcu xdebug \
      && docker-php-ext-enable apcu

ENV DOCKERIZE_VERSION v0.6.1
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === '93b54496392c062774670ac18b134c3b3a95e5a5e5c8f1a9f115f203b75bf9a129d5daa8ba6a13e2cc8a1da0806388a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer

ENV COMPOSER_BIN_DIR /usr/local/bin
ENV COMPOSER_ALLOW_SUPERUSER 1
