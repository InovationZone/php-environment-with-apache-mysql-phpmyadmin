FROM php:7.4-apache
MAINTAINER João Silva

RUN apt-get update && apt-get install -y \
    vim \
    git \ 
    unzip \
    iputils-ping \
    procps \
    libzip-dev \
    zip \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libxml2-dev

RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

RUN docker-php-ext-install gd intl pdo_mysql pdo_pgsql mysqli zip \
  && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

#install composer plugin for parallel installs
RUN composer global require hirak/prestissimo

# Clear package lists
RUN apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Permissions
RUN chown -R root:www-data /var/www/html
RUN chmod u+rwx,g+rx,o+rx /var/www/html
RUN find /var/www/html -type d -exec chmod u+rwx,g+rx,o+rx {} +
RUN find /var/www/html -type f -exec chmod u+rw,g+rw,o+r {} +

WORKDIR /var/www/html

RUN a2enmod rewrite
RUN a2enmod ssl

EXPOSE 80
EXPOSE 443