FROM php:7-fpm-alpine

RUN apk upgrade -U -a
RUN apk add --update freetype libjpeg-turbo libpng
RUN apk add --update libmcrypt
RUN apk add --update libbz2
RUN apk add --update minizip
RUN apk add --update mysql-client

RUN apk add --virtual=build-dep alpine-sdk autoconf \
  freetype-dev libjpeg-turbo-dev libpng-dev \
  libmcrypt-dev bzip2-dev minizip-dev openssl-dev

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
  apcpath=`ls /usr/local/lib/php/extensions/no-debug-non-zts-*/apcu.so` &&  \
  chmod 755 $apcpath && \
  echo "extension=$apcpath" > /usr/local/etc/php/conf.d/apcu.ini


# Ubuntuホストとwww-dataのuid/gidをあわせる
RUN deluser www-data && \
  deluser xfs && \
  addgroup -g 33 -S www-data && \
  adduser -u 33 -D -S -G www-data www-data

RUN apk del --purge build-dep
RUN rm -rf /var/cache/apk/*

COPY conf.d/*.ini /usr/local/etc/php/conf.d/
