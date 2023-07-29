FROM php:8.1.12-apache

ENV NODE_VERSION=18.x
ENV NPM_VERSION=9.8.1
ENV COMPOSER_VERSION=2.5.5
ENV LARAVEL_VERSION=10.10

WORKDIR /var/www/html

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
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install mysqli mysqli pdo pdo_mysql \
    && docker-php-ext-install bz2 \
    && a2enmod rewrite \
    && a2enmod vhost_alias

RUN curl -fsSL https://deb.nodesource.com/setup_${NODE_VERSION} | bash -
RUN apt-get install -y nodejs
RUN apt-get update
RUN npm install -g npm@${NPM_VERSION}

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --version=${COMPOSER_VERSION}
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer
RUN composer global require laravel/installer
RUN pecl install mongodb

ENV PATH $PATH:~/.composer/vendor/bin
