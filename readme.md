# alpha-docker 

本项目基于 Docker，Docker 是一个优秀的开源项目，它基于虚拟化技术，可以极大的提高应用的维护效率，降低云计算应用开发的成本,使用 Docker 可以让应用的部署、测试和分发都变得前所未有的高效和轻松！
这个项目就是基于 Docker 的 compose 实现，通过编写 docker-compose.yml 和 Dockerfile ，我们可以一键打包并部署一个完整的 lnmp 项目。
Docker 的官网地址是 https://www.docker.com/ 还没有上船小伙伴们抓紧咯！

## 镜像列表

``nginx + php + mysql + redis``

```
nginx: 1.14
php: 7.2(已安装 composer)
mysql: 5.7
redis: 3.2
```

## 使用

1. linux 服务器上已经安装了 docker, docker-compose

2. 启动项目

    ```
    cd ..alpha-docker/docker # 进入 docker-compose.yml 文件的目录
    docker-compose up -d # 构建镜像，启动容器 -d 表示守护模式启动容器
    
    
    
    docker-compose down # 关闭容器
    ```
3. 浏览器访问

    http://www.alpha.com/ # 配置好host 文件，将改域名指向 linux 服务器地址
