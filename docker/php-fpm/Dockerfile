FROM php:7.2-fpm
LABEL maintainer="security.2009@live.cn"

#  设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 设置 apt 镜像源
COPY sources.list /etc/apt/sources.list

# 更新安装依赖包和 PHP 核心扩展
RUN apt-get update && apt-get install -y --no-install-recommends \
        git \
        curl \
        wget \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libjpeg-dev \
        libicu-dev \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libssl-dev \
        libmcrypt-dev \
        libxml2-dev \
        libbz2-dev \
        bzip2 \
        libmagickwand-dev \
    # PHP 核心扩展
    && docker-php-ext-configure gd --enable-gd-native-ttf --with-jpeg-dir=/usr/lib --with-freetype-dir=/usr/include/freetype2 \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql \
    && docker-php-ext-configure pdo_pgsql --with-pgsql \
    && docker-php-ext-configure mbstring --enable-mbstring \
    && docker-php-ext-configure soap --enable-soap \
    && docker-php-ext-install -j$(nproc) gd zip pdo_mysql pdo_pgsql opcache mysqli soap sockets

# 安装 PECL 扩展
RUN pecl install imagick \
    && pecl install xdebug-2.6.0 \
    && pecl install redis-4.0.1 \
    && pecl install memcached-3.0.4 \
    && docker-php-ext-enable imagick xdebug redis memcached

#  安装第三方扩展，
# Phalcon
# swoole

# 安装 Composer
RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com

# apt-cache clean
RUN apt-get autoremove -y \
    && apt-get autoclean -y \
    && apt-get clean -y \
    && rm -rf /var/cache/apt/* \
    && rm -rf /var/lib/apt/lists/*

 WORKDIR /data

 # Write Permission
 RUN usermod -u 1000 www-data