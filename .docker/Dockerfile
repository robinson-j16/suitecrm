FROM php:7.4-apache

RUN apt-get update -y && apt-get install -y libbz2-dev libpng-dev libmcrypt-dev libxml2-dev libc-client-dev libkrb5-dev libmemcached-dev

RUN rm -r /var/lib/apt/lists/*

RUN apt-get install libmcrypt-dev
RUN pecl install mcrypt-1.0.4 && docker-php-ext-enable mcrypt

RUN apt-get update \
     && apt-get install -y libzip-dev \
     && docker-php-ext-install zip

RUN docker-php-ext-install bz2        \
    && docker-php-ext-install gd      \
    && docker-php-ext-install mysqli


RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
   && docker-php-ext-install zip imap intl pdo pdo_mysql gd

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN pecl install memcached

RUN echo "extension=memcached.so" > /usr/local/etc/php/conf.d/20-memcached.ini

RUN a2enmod rewrite && a2enmod headers

ADD ./server/001-sugarlocal.conf /etc/apache2/sites-available/001-sugarlocal.conf
ADD ./server/50-suitecrm.ini /usr/local/etc/php/conf.d/50-suitecrm.ini

RUN a2ensite 001-sugarlocal.conf

RUN chown -R www-data:www-data /var/www
