FROM php:7.2-fpm
LABEL maintainer="mingyangdu security.2009@live.cn"

# PHP扩展包版本(使用网络直接下载时放开注释)
#ENV PHALCON_VERSION=3.4.0
#ENV IMAGICK_VERSION=3.4.3
#ENV REDIS_VERSION=4.1.1
#ENV MEMCACHED_VERSION=3.0.4
#ENV YAF_VERSION=3.0.7
#ENV SWOOLE_VERSION=4.1.2
#ENV XDEBUG_VERSION=2.6.1

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 设置 apt 镜像源
COPY ./docker/sources.list /etc/apt/sources.list
# 暂时弃用 Debian buster 软件包(可以不弃用，但更新较慢，暂时没有使用到该软件源安装软件)
RUN mv /etc/apt/sources.list.d/buster.list /etc/apt/sources.list.d/buster.list.bak

# 使用本地已下载好的扩展包，节省下载时间，网络下载受墙的原因经常下载慢
#COPY ./docker/php-fpm/packages/imagick-3.4.3.tgz /home/imagick.tgz
#COPY ./docker/php-fpm/packages/memcached-3.0.4.tgz /home/memcached.tgz
COPY ./docker/php-fpm/packages/redis-4.1.1.tgz /home/redis.tgz
#COPY ./docker/php-fpm/packages/xdebug-2.6.1.tgz /home/xdebug.tgz
#COPY ./docker/php-fpm/packages/swoole-4.1.2.tgz /home/swoole.tgz
#COPY ./docker/php-fpm/packages/yaf-3.0.7.tgz /home/yaf.tgz
#COPY ./docker/php-fpm/packages/cphalcon.tar.gz /home/cphalcon.tar.gz

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
    # 安装 PECL 扩展(网络直接下载时放开注释)，记得放开ENV中的版本号声明
    # && pecl install redis-${REDIS_VERSION} && docker-php-ext-enable redis \
    # && pecl install imagick-${IMAGICK_VERSION} && docker-php-ext-enable imagick \
    # && pecl install memcached-${MEMCACHED_VERSION} && && docker-php-ext-enable memcached \
    # && pecl install yaf-${YAF_VERSION} && docker-php-ext-enable yaf\
    # && pecl install swoole-${SWOOLE_VERSION} && docker-php-ext-enable swoole \
    # && pecl install xdebug-${XDEBUG_VERSION} && docker-php-ext-enable xdebug\
    # 使用本地包安装 PECL 扩展,本地包或在线下载根据你的网络情况只能选其一
    && pecl install /home/redis.tgz && docker-php-ext-enable redis && rm -rf /home/redis.tgz \
#    && pecl install /home/imagick.tgz && docker-php-ext-enable imagick && rm -rf /home/imagick.tgz \
#    && pecl install /home/memcached.tgz && docker-php-ext-enable memcached && rm -rf /home/memcached.tgz \
#    && pecl install /home/swoole.tgz && docker-php-ext-enable swoole && rm -rf /home/swoole.tgz \
#    && pecl install /home/yaf.tgz && docker-php-ext-enable yaf && rm -rf /home/yaf.tgz \
#    && pecl install /home/xdebug.tgz && docker-php-ext-enable xdebug && rm -rf /home/xdebug.tgz \
    # xdebug 配置
#    && echo "xdebug.remote_enable=on\n" >> $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}')/docker-php-ext-xdebug.ini \
    # 安装第三方扩展 phalcon(在线下载)，记得放开ENV中的版本号声明
    # && curl -sSL "https://codeload.github.com/phalcon/cphalcon/tar.gz/v${PHALCON_VERSION}" | tar -xz \
    # && cd cphalcon-${PHALCON_VERSION}/build && ./install \
    # && cp ../tests/_ci/phalcon.ini $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}') \
    # && cd ../../ \
    # && rm -r cphalcon-${PHALCON_VERSION} \
    # 安装第三方扩展 phalcon(使用本地包),本地包或在线下载根据你的网络情况只能选其一
#     && cd /home \
#     && tar -zxvf cphalcon.tar.gz \
#     && cd cphalcon/build \
#     && ./install \
#     && cp ../tests/_ci/phalcon.ini $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}') \
#     && cd ../../ \
#     && rm -rf cphalcon \
#     && rm -rf /home/cphalcon.tar.gz \
     # 安装 Composer
     && php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" \
     && php composer-setup.php \
     && php -r "unlink('composer-setup.php');" \
     && mv composer.phar /usr/local/bin/composer \
     # Composer 中国镜像源
     && composer config -g repo.packagist composer https://packagist.phpcomposer.com \
     # Laravel 中文社区镜像源(Composer 镜像源地址选其一)
     # && composer config -g repo.packagist composer https://packagist.laravel-china.org \
     # 清理 apt 安装
     && apt-get autoremove -y \
     && apt-get autoclean -y \
     && apt-get clean -y \
     && rm -rf /var/cache/apt/* \
     && rm -rf /var/lib/apt/lists/*

WORKDIR /data
#COPY ./app/alpha/ ./

# Write Permission
RUN usermod -u 1000 www-data