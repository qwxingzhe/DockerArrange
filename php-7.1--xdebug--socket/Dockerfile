FROM php:7.1-fpm

COPY ./sources.list /etc/apt/sources.list

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg-dev \
        libpng-dev \
        libmcrypt-dev \
    && docker-php-ext-install zip \
    && docker-php-ext-install -j$(nproc) pdo_mysql mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && usermod -u 1000 www-data \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && pecl install swoole \
    && docker-php-ext-enable swoole

# Copy xdebug configration for remote debugging
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

ADD ./php.ini /usr/local/etc/php/
ADD ./xlaravel.pool.conf /usr/local/etc/php-fpm.d/

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
