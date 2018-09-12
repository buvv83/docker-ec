FROM php:7.2-fpm

RUN apt-get update

RUN apt-get install -y \
        libzip-dev \
        zip \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
  && docker-php-ext-configure zip --with-libzip \
  && docker-php-ext-install zip \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install -j$(nproc) gd

RUN docker-php-ext-install bcmath pdo_mysql

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer




