FROM php:7.2-fpm

RUN apt-get update

RUN apt-get install -y \
        libzip-dev \
        zip \
  && docker-php-ext-configure zip --with-libzip \
  && docker-php-ext-install zip

RUN docker-php-ext-install bcmath pdo_mysql

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer




