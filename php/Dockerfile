FROM php:7-fpm-alpine

RUN apk upgrade -U -a
RUN apk add --update freetype libjpeg-turbo libpng
RUN apk add --update libmcrypt
RUN apk add --update libbz2
RUN apk add --update minizip

RUN apk add --virtual=build-dep alpine-sdk autoconf \
  freetype-dev libjpeg-turbo-dev libpng-dev \
  libmcrypt-dev bzip2-dev minizip-dev

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include --with-jpeg-dir=/usr/include
RUN docker-php-ext-install \
      mbstring \
      mysqli \
      pdo_mysql \
      opcache \
      gd \
      mcrypt \
      zip \
      bz2 \
      ftp \
      pcntl

RUN pecl install apcu && \
  chmod 755 /usr/local/lib/php/extensions/no-debug-non-zts-20151012/apcu.so && \
  echo "extension=/usr/local/lib/php/extensions/no-debug-non-zts-20151012/apcu.so" > /usr/local/etc/php/conf.d/apcu.ini


# Ubuntuホストとwww-dataのuid/gidをあわせる
RUN deluser www-data && \
  deluser xfs && \
  addgroup -g 33 -S www-data && \
  adduser -u 33 -D -S -G www-data www-data

RUN apk del --purge build-dep
RUN rm -rf /var/cache/apk/*

COPY conf.d/*.ini /usr/local/etc/php/conf.d/
