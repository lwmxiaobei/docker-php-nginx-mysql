FROM php:7.3.9-fpm

LABEL maintainer="xiaobei <806641926@qq.com>"

# ENV for Global
ENV TZ=Asia/Shanghai
ENV DEPENDENCIES curl gnupg git wget gcc g++

# ENV for PHP
ENV PHP_DEPENDENCIES \
                    unzip libwebp-dev libmcrypt-dev libmemcached-dev libbz2-dev libpng-dev \
                    libxpm-dev librabbitmq-dev libfreetype6-dev libjpeg-dev libssh-dev libzip-dev
ENV PHP_EXT_INSTALLED \
                    mongodb-1.6.0 swoole redis-4.3.0 memcached-3.1.3 mcrypt-1.0.3
ENV PHP_COMPOSER_URL https://getcomposer.org/composer.phar
ENV PHP_COMPOSER_REPO https://mirrors.aliyun.com/composer/

# INSTALL PHP
RUN apt-get update \
  && apt-get install -y $PHP_DEPENDENCIES $DEPENDENCIES \
  && pecl install -o -f $PHP_EXT_INSTALLED \
  && docker-php-ext-configure gd --with-gd --with-webp-dir --with-jpeg-dir \
      --with-png-dir --with-zlib-dir --with-xpm-dir --with-freetype-dir \
  && docker-php-ext-install opcache bcmath bz2 gd iconv mysqli pdo pdo_mysql zip sockets \
  && docker-php-ext-enable opcache redis memcached mongodb swoole mcrypt \
  && curl $PHP_COMPOSER_URL -o /usr/local/bin/composer \
  && chmod a+x /usr/local/bin/composer \
  && composer config -g repo.packagist composer $PHP_COMPOSER_REPO \
  && cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini \
# CLEAN
  && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
  && apt-get -y remove $DEPENDENCIES \
  && apt-get purge -y --auto-remove \
  && rm -rf /var/lib/apt/lists/* \
  && rm -rf /tmp/pear ~/.pearrc

CMD php-fpm