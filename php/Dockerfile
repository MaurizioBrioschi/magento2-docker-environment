FROM php:7.2.20-fpm-alpine3.10

MAINTAINER Maurizio Brioschi<maurizio.brioschi@gmail.com>

# Create our non-priviledged user that will run magento.
RUN addgroup -S magento && adduser -S -h /magento -s /bin/sh -G magento magento

RUN apk --update --no-cache add \
    wget \
    gcc build-base \
    freetype \
    libpng \
    libjpeg-turbo \
    freetype-dev \
    libpng-dev \
    jpeg-dev \
    libjpeg \
    libjpeg-turbo-dev \
    libintl icu icu-dev pkgconfig zlib-dev libxslt libxml2-dev \
    php7-common \
    php7-ctype \
    php7-curl \
    php7-dom \
    php7-iconv \
    php7-json \
    php7-mbstring \
    php7-mcrypt \
    php7-openssl \
    php7-pdo_mysql \
    php7-phar \
    php7-session \
    php7-simplexml \
    php7-tokenizer \
    php7-xmlwriter \
    php7-xsl \
    php7-zlib

RUN cd /tmp && wget http://xmlsoft.org/sources/libxslt-1.1.33.tar.gz \
    && tar -zxvf libxslt-1.1.33.tar.gz && cd libxslt-1.1.33 \
    && wget  http://www.linuxfromscratch.org/patches/blfs/svn/libxslt-1.1.33-security_fix-1.patch \
    && patch -Np1 -i libxslt-1.1.33-security_fix-1.patch \
    && ./configure --prefix=/usr --disable-static && make && make install \
    && apk del gcc build-base wget

RUN docker-php-ext-configure gd \
            --with-freetype-dir=/usr/lib/ \
            --with-png-dir=/usr/lib/ \
            --with-jpeg-dir=/usr/lib/ \
            --with-gd \
    && NUMPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && docker-php-ext-install -j${NUMPROC} gd  mysqli pdo pdo_mysql bcmath intl zip opcache xml soap xsl

COPY src/ /var/www/html/magento2

WORKDIR  /var/www/html/magento2