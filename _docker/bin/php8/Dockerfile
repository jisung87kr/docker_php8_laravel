FROM php:8.0.3-apache

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

RUN apt-get install -y nodejs \
    npm

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer
RUN composer global require laravel/installer

RUN cd /var/www/html
#RUN arch=$(arch | sed s/aarch64/arm64/ | sed s/x86_64/amd64/) \
#    && wget "https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.buster_${arch}.deb"  \
#    && dpkg -i "wkhtmltox_0.12.6-1.buster_${arch}.deb"
#
#RUN curl -o nanumfont.zip http://cdn.naver.com/naver/NanumFont/fontfiles/NanumFont_TTF_ALL.zip
#RUN unzip -d /usr/share/fonts/nanum nanumfont.zip
#RUN fc-cache -f -v

ENV PATH $PATH:~/.composer/vendor/bin
