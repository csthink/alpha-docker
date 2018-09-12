FROM php:7.2-fpm
LABEL maintainer="mingyangdu security.2009@live.cn"

# PHP扩展包版本
ENV PHALCON_VERSION=3.4.0
ENV IMAGICK_VERSION=3.4.3
ENV REDIS_VERSION=4.0.1
ENV MEMCACHED_VERSION=3.0.4
ENV YAF_VERSION=3.0.7
ENV SWOOLE_VERSION=4.1.2
ENV XDEBUG_VERSION=2.6.1

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 设置 apt 镜像源
COPY ./docker/sources.list /etc/apt/sources.list

# 更新安装依赖包和 PHP 核心扩展
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    vim \
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
    libgd2-xpm-dev* \
    # PHP 核心扩展
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql \
    && docker-php-ext-configure mbstring --enable-mbstring \
    && docker-php-ext-configure soap --enable-soap \
    && docker-php-ext-install -j$(nproc) gd opcache zip pdo_mysql mysqli soap sockets \
    # 安装 PECL 扩展
    && pecl install imagick-${IMAGICK_VERSION} redis-${REDIS_VERSION} memcached-${MEMCACHED_VERSION} yaf-${YAF_VERSION} swoole-${SWOOLE_VERSION} xdebug-${XDEBUG_VERSION} \
    && docker-php-ext-enable imagick redis memcached yaf swoole xdebug \
    # xdebug 配置
    && echo "xdebug.remote_enable=on\n" >> $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}')/docker-php-ext-xdebug.ini \
    # 安装第三方扩展 phalcon
    && curl -sSL "https://codeload.github.com/phalcon/cphalcon/tar.gz/v${PHALCON_VERSION}" | tar -xz \
    && cd cphalcon-${PHALCON_VERSION}/build && ./install \
    && cp ../tests/_ci/phalcon.ini $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}') \
    && cd ../../ \
    && rm -r cphalcon-${PHALCON_VERSION} \
    # 安装 Composer
    && php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com \
    && apt-get autoremove -y \
    && apt-get autoclean -y \
    && apt-get clean -y \
    && rm -rf /var/cache/apt/* \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /data/www/
COPY ./app/alpha/ ./

RUN find storage -type d -exec chmod 777 {} \;
RUN chmod 777 bootstrap/cache;
RUN composer install && php artisan key:generate && php artisan migrate:refresh

# Write Permission
RUN usermod -u 1000 www-data