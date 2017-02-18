FROM php:fpm
MAINTAINER helbing <helbingxxx@gmail.com>

# 切换sh脚本指向，从dash修改为bash
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# 配置镜像源
ARG APT_REPOSITORY=false
ENV APT_REPOSITORY ${APT_REPOSITORY}

RUN if [ ${APT_REPOSITORY} != false ]; then \

    cp /etc/apt/sources.list /etc/apt/sources.list.bak && \
    sed -i "s%archive.ubuntu.com%$APT_REPOSITORY%g" /etc/apt/sources.list \
;fi

# 安装一些包
RUN apt-get -y update && \
    apt-get install -y apt-utils zlib1g-dev pkg-config libssl-dev libmcrypt-dev

# 加载一些必要的拓展
RUN docker-php-ext-install mcrypt && \
    docker-php-ext-install pdo_mysql

# 安装xdebug拓展
ARG XDEBUG_ENABLE=false

RUN if [ ${XDEBUG_ENABLE} = true ]; then \

    pecl install xdebug && docker-php-ext-enable xdebug \
;fi

# 安装gd拓展
ARG GD_ENABLE=false

RUN if [ ${GD_ENABLE} = true ]; then \

    apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev && \
    docker-php-ext-install -j$(nproc) iconv mcrypt && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install -j$(nproc) gd \
;fi

# 安装soap拓展
ARG SOAP_ENABLE=false

RUN if [ ${SOAP_ENABLE} = true ]; then \

    apt-get -y install libxml2-dev && \
    docker-php-ext-install soap \
;fi

# 安装zip拓展
ARG ZIP_ENABLE=false

RUN if [ ${ZIP_ENABLE} = true ]; then \

    pecl install zip && docker-php-ext-enable zip \
;fi

# 安装redis拓展
ARG REDIS_ENABLE=false

RUN if [ ${REDIS_ENABLE} = true ]; then \

    pecl install redis && docker-php-ext-enable redis \
;fi

# 安装memcached拓展
ARG MEMCACHED_ENABLE=false

RUN if [ ${MEMCACHED_ENABLE} == true ]; then \

    apt-get install -y libmemcached-dev && \
    curl -L -o /tmp/memcached.tar.gz "https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz" && \
    mkdir -p memcached && \
    tar -C memcached -zxvf /tmp/memcached.tar.gz --strip 1 && \
    ( \
        cd memcached && \
        phpize && \
        ./configure && \
        make -j$(nproc) && \
        make install \
    ) && \
    rm -r memcached && \
    rm /tmp/memcached.tar.gz && \
    docker-php-ext-enable memcached \
;fi

# 安装mongodb拓展
ARG MONGO_ENABLE=false

RUN if [ ${MONGO_ENABLE} = true ]; then \

    pecl install mongodb && \
    docker-php-ext-enable mongodb \
;fi

# 安装imagick拓展
ARG IMAGICK_ENABLE=false

RUN if [ ${IMAGICK_ENABLE} = true ]; then \

    apt-get install -y imagemagick libmagickwand-dev && \
    pecl install imagick && \
    docker-php-ext-enable imagick \
;fi

# 安装opcahce拓展
ARG OPCACHE_ENABLE=false

RUN if [ ${OPCACHE_ENABLE} = true ]; then \

    docker-php-ext-enable opcache \
;fi

ARG POSTGRESQL_ENABLE=false

RUN if [ ${POSTGRESQL_ENABLE} = true ]; then \

    apt-get install -y libpq-dev && \
    docker-php-ext-install pdo_pgsql \
;fi
EXPOSE 9000
