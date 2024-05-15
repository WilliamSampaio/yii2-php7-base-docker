FROM php:7-apache

ENV DEBIAN_FRONTEND=noninteractive

# install dependencies
RUN apt-get update
RUN apt-get install -y build-essential vim

# pdo extension
RUN docker-php-ext-install -j$(nproc) pdo

# mbstring extension
RUN apt-get install -y libonig-dev \
  && docker-php-ext-install -j$(nproc) mbstring

# intl extension
RUN apt-get install -y libicu-dev \
  && docker-php-ext-configure intl \
  && docker-php-ext-install -j$(nproc) intl

# zip extension
RUN apt-get install -y libzip-dev unzip zip \
  && docker-php-ext-configure zip \
  && docker-php-ext-install -j$(nproc) zip

# mysql extension
RUN apt-get install -y default-mysql-client \
  && docker-php-ext-configure mysqli --with-mysqli=mysqlnd \
  && docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
  && docker-php-ext-install -j$(nproc) mysqli pdo_mysql

# postgresql extension
RUN apt-get install -y libpq-dev \
  && docker-php-ext-install -j$(nproc) pgsql pdo_pgsql

# pcntl extension
RUN docker-php-ext-configure pcntl --enable-pcntl \
  && docker-php-ext-install pcntl

# gd extension
RUN apt-get install -y libgd-dev \
  && docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg \
  && docker-php-ext-install -j$(nproc) gd

# memcached extension
RUN apt-get install -y libmemcached-dev \
  && pecl install memcached-3.1.5 \
  && docker-php-ext-enable memcached

# imagick extension
RUN apt-get install -y libmagickwand-dev \
  && pecl install imagick \
  && docker-php-ext-enable imagick

# install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN a2enmod rewrite
RUN a2enmod headers

COPY app.conf /etc/apache2/sites-available/app.conf

RUN a2dissite 000-default.conf
RUN a2ensite app.conf

COPY php.ini /usr/local/etc/php/conf.d/conf.ini

# clean image
RUN apt-get clean
