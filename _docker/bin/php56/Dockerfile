FROM php:5.6-apache

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        vim \
        zip \
        libfontconfig \
        zlib1g \
        libxrender1 \
        libxext6 \
        libx11-6 \
        fontconfig \
        xfonts-75dpi \
        xfonts-base \
        wget \
        git \
        imagemagick \
        libmagickwand-dev \
        libmagickcore-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install mysqli mysqli pdo pdo_mysql \
    && docker-php-ext-install bz2 \
    && a2enmod rewrite \
    && a2enmod vhost_alias
