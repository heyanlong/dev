FROM php:7.1.12-fpm-alpine

ENV BUILD_DEPS autoconf g++ gcc make file

COPY config/php.ini /usr/local/etc/php/

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories

RUN set -ex \
    && apk update && apk add --no-cache --virtual .build-deps $BUILD_DEPS \
    && wget -q "https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-3.4.11/zookeeper-3.4.11.tar.gz" \
    && tar zxf zookeeper-3.4.11.tar.gz \
    && cd zookeeper-3.4.11/src/c \
    && ./configure && make && make install \
    && cd ../../../ && rm -fr zookeeper-3.4.11.tar.gz zookeeper-3.4.11 \
    && pecl install zookeeper-0.4.0 yaf-3.0.6 && docker-php-ext-enable zookeeper yaf \
    && apk del .build-deps \
    && rm -fr /tmp/pear

RUN set -ex \
    && apk update && apk add freetype-dev libjpeg-turbo-dev libpng-dev \
    && docker-php-ext-install pdo_mysql bcmath \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && rm -fr /var/cache/apk/*