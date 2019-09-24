FROM php:7.3-apache

ARG URL

ENV APACHE_DOCUMENT_ROOT /var/www/html/${URL}/public
ENV COMPOSER_MEMORY_LIMIT "-1"

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN apt-get update && apt-get install -y \
    libcap2-bin \
    libzip-dev \
    zip \
    unzip \
    git \
    && docker-php-ext-install -j$(nproc) mysqli pdo pdo_mysql zip

RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

RUN mkdir /var/www/.composer && chown www-data: /var/www/.composer

RUN a2enmod rewrite

RUN setcap 'cap_net_bind_service=+ep' /usr/sbin/apache2

USER www-data
