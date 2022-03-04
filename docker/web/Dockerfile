FROM php:5.6-apache

COPY ./php.ini /usr/local/etc/php/php.ini
RUN apt update && apt install -y \
   vim \
   zip \
   unzip \
   iputils-ping \
   git \
   wget \
   zlib1g \
   zlib1g-dev \
   libpng-dev \
   libjpeg-dev \
   libpq-dev \
   libfreetype6-dev \
        # && a2enmod ldap && a2enmod authnz_ldap \
        && docker-php-ext-install mysql \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-configure gd --with-freetype --with-jpeg \
        && docker-php-ext-install -j$(nproc) gd
