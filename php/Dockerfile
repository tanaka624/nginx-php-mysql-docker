FROM php:7-fpm

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y libfreetype6 libjpeg62-turbo libpng12-0
RUN apt-get install -y libmcrypt4
RUN apt-get install -y bzip2
RUN apt-get install -y libzip2
RUN apt-get install -y libssl1.0.0
RUN apt-get install -y esmtp
RUN apt-get install -y mysql-client

RUN apt-get install -y autoconf \
  libfreetype6-dev libjpeg62-turbo-dev libpng12-dev \
  libmcrypt-dev libbz2-dev libzip-dev libssl-dev

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
  apcpath=`ls /usr/local/lib/php/extensions/no-debug-non-zts-*/apcu.so` && \
  chmod 755 $apcpath && \
  echo "extension=$apcpath" > /usr/local/etc/php/conf.d/apcu.ini

RUN apt-get install -y tzdata
RUN cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
RUN apt-get remove -y autoconf \
  libfreetype6-dev libjpeg62-turbo-dev libpng12-dev \
  libmcrypt-dev libbz2-dev libzip-dev libssl-dev
RUN apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

COPY conf.d/*.ini /usr/local/etc/php/conf.d/
COPY otherconf/inputrc /root/.inputrc
COPY otherconf/esmtprc /etc/
