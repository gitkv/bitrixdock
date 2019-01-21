FROM php:7.2-fpm-alpine
LABEL Maintainer="Nikolay Volkov <gitkv@ya.ru>" \
      Description="PHP-FPM 7.2 & Composer based on Alpine Linux."

ENV COMPOSER_ALLOW_SUPERUSER 1

# Add php configs
ADD ./php.ini /etc/php/7.2/fpm/conf.d/999z-php.ini
ADD ./php.ini /etc/php/7.2/cli/conf.d/999z-php.ini

# Install dependences
RUN set -x \
    && apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS imagemagick-dev libtool \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && apk add --no-cache --virtual .imagick-runtime-deps imagemagick \
    && apk del .phpize-deps \

    && echo http://dl-2.alpinelinux.org/alpine/edge/community/ >> /etc/apk/repositories \
    && apk --no-cache add libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
    && docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-png-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && docker-php-ext-install -j${NPROC} mbstring pdo mysqli pdo_mysql intl interbase soap gd opcache zip \
#    todo: memcache xdebug mcrypt
    && apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev \
    && rm -rf /var/cache/apk/* /tmp/* \
    && usermod -u 1000 www-data

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer \
    && composer global require hirak/prestissimo

WORKDIR /var/www/app

EXPOSE 9000
