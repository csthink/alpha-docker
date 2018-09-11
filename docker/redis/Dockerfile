FROM redis:3.2
LABEL maintainer="security.2009@live.cn"

#  set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone