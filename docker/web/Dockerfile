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
        && docker-php-ext-install mysql \
        && docker-php-ext-configure gd --with-freetype --with-jpeg \
        && docker-php-ext-install -j$(nproc) gd \
        && echo "file_uploads = On\n" \
         "memory_limit = 500M\n" \
         "upload_max_filesize = 500M\n" \
         "post_max_size = 500M\n" \
         "max_execution_time = 600\n" \
         > /usr/local/etc/php/conf.d/uploads.ini
