FROM node:8.11.4
LABEL maintainer="mingyangdu security.2009@live.cn"

#  set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 设置 apt 镜像源
COPY ./docker/sources.list /etc/apt/sources.list

# 更新安装依赖包和 PHP 核心扩展
RUN apt-get update && apt-get install -y --no-install-recommends yarn \
    && apt-get autoremove -y \
    && apt-get autoclean -y \
    && apt-get clean -y \
    && rm -rf /var/cache/apt/* \
    && rm -rf /var/lib/apt/lists/*

RUN npm config set registry https://registry.npm.taobao.org

WORKDIR /data/www/
COPY ./app/alpha/ ./

RUN yarn config set registry 'https://registry.npm.taobao.org' \
    && yarn install --no-bin-links \
    && yarn add cross-env \
    && npm run watch-poll
