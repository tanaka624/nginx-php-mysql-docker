# FROM php:7.4-fpm
FROM php:8.0-fpm

# Install packages
RUN apt-get update && apt-get install -y \
  bzip2 \
  default-mysql-client \
  esmtp \
  libfreetype6 \
  libjpeg62-turbo \
  && rm -rf /var/lib/apt/lists/*

# Remove after Install php extention
RUN apt-get update && apt-get install -y \
  autoconf \
  libbz2-dev \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  libonig-dev \
  libpng-dev \
  libssl-dev \
  libzip-dev \
  && rm -rf /var/lib/apt/lists/*

# Install php extentions
RUN docker-php-ext-configure gd --with-freetype=/usr/include --with-jpeg=/usr/include \
  && docker-php-ext-install -j$(nproc) \
  bz2 \
  ftp \
  gd \
  mbstring \
  mysqli \
  opcache \
  pcntl \
  pdo_mysql \
  zip

# Install php extention apcu
RUN pecl install apcu \
  && apcpath=`ls /usr/local/lib/php/extensions/no-debug-non-zts-*/apcu.so` \
  && chmod 755 $apcpath \
  && echo "extension=$apcpath" > /usr/local/etc/php/conf.d/apcu.ini

# Install php extention xdebug
RUN pecl install xdebug \
  && docker-php-ext-enable xdebug

# Set localtime
RUN apt-get update && apt-get install -y tzdata \
  && cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# Set locale
RUN apt-get update && apt-get install -y \
  locales \
  && rm -rf /var/lib/apt/lists/*
RUN sed -i -E 's/# (ja_JP.UTF-8)/\1/' /etc/locale.gen \
  && locale-gen
ENV LANG ja_JP.UTF-8

# Remove packages
RUN apt-get remove -y \
  autoconf \
  libbz2-dev \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  libonig-dev \
  libpng-dev \
  libssl-dev \
  libzip-dev

RUN apt-get clean \
  && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

# Copy config files
COPY ./conf.d/*.ini /usr/local/etc/php/conf.d
COPY ./otherconf/inputrc /root/.inputrc
COPY ./otherconf/esmtprc /etc/
