FROM php:7.4-apache

ENV TERM=xterm \
    PNGOUT_VERSION=20200115 \
    IMAGICK_VERSION=3.4.4 \
    COMPOSER_ALLOW_SUPERUSER=1

# Install base requirements & sensible defaults + required PHP extensions
RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        libmagickwand-dev \
        libjpeg-dev \
        libxml2-dev \
        ghostscript \
        libpng-dev \
        gifsicle \
        optipng \
        libjpeg-turbo-progs \
        sudo \
        zip \
        unzip \
        libzip-dev \
        git \
        mariadb-client \
    && rm -rf /var/lib/apt/lists/* \

    # Pngout
    && curl -OL https://www.jonof.id.au/files/kenutils/pngout-${PNGOUT_VERSION}-linux-static.tar.gz \
    && tar xvf pngout-${PNGOUT_VERSION}-linux-static.tar.gz \
    && mv pngout-${PNGOUT_VERSION}-linux-static/i686/pngout-static /usr/local/bin/pngout \
    && rm -rf pngout-${PNGOUT_VERSION}-linux-static* \

    # PHP moduels
    && docker-php-ext-configure gd \
        --with-freetype \
        --with-jpeg \
    && docker-php-ext-install \
        exif \
        gd \
        mysqli \
        pdo_mysql \
        opcache \
        soap \
        zip \
        bcmath \
    && echo "extension=imagick.so" > /usr/local/etc/php/conf.d/ext-imagick.ini \
    && pecl install -f xdebug imagick-${IMAGICK_VERSION} \

    # Apache modules
    && a2enmod rewrite expires headers deflate \
    && a2dismod -f autoindex

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

RUN curl -o /usr/local/bin/wp https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x /usr/local/bin/wp \

    # Install Prestissimo (pallarell installation)
    && composer global require hirak/prestissimo \

    # Basic config
    && { \
        echo 'ServerTokens Prod'; \
        echo 'ServerSignature Off'; \
        echo 'TraceEnable Off'; \
    } > /etc/apache2/conf-available/security.conf