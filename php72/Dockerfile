FROM php:7.2-fpm-alpine
LABEL Maintainer="Nikolay Volkov <gitkv@ya.ru>" \
      Description="PHP-FPM 7.2 & Composer based on Alpine Linux."

ENV COMPOSER_ALLOW_SUPERUSER 1

# Install dependences
RUN echo http://dl-2.alpinelinux.org/alpine/edge/community/ >> /etc/apk/repositories \
    && apk --no-cache add shadow libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev libxml2-dev \
    && docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-png-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure xml --with-libxml-dir=/usr/include/ \
    && docker-php-ext-install gd mysqli pdo pdo_mysql mbstring opcache xml zip \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer \
    && composer global require hirak/prestissimo \
    && rm -rf /var/cache/apk/* /tmp/* \
    && usermod -u 1000 www-data

# Add php config
ADD ./php.ini $PHP_INI_DIR/conf.d/999z-php.ini

WORKDIR /var/www/app

EXPOSE 9000
