FROM letsdeal/php:8.0-apache

RUN apt-get update && apt-get upgrade -y \
  && apt-get install -y fontconfig fonts-roboto libjpeg62-turbo libxrender1 xfonts-75dpi xfonts-base \
  && apt-get install -y fonts-roboto-slab

RUN curl -LO https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.stretch_amd64.deb \
 && dpkg -i wkhtmltox_0.12.6-1.stretch_amd64.deb \
 && rm wkhtmltox_0.12.6-1.stretch_amd64.deb
