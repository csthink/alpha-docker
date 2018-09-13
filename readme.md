# alpha-docker 

本项目基于 Docker，Docker 是一个优秀的开源项目，它基于虚拟化技术，可以极大的提高应用的维护效率，降低云计算应用开发的成本,使用 Docker 可以让应用的部署、测试和分发都变得前所未有的高效和轻松！
这个项目就是基于 Docker 的 compose 实现，通过编写 docker-compose.yml 和 Dockerfile ，我们可以一键打包并部署一个完整的 lnmp 项目。
Docker 的官网地址是 https://www.docker.com/ 还没有上船小伙伴们抓紧咯！

## 环境

* CentOS Linux release 7.5.1804 (Core)
* Nginx: nginx-1.14
* PHP: php-7.2.9
* MySQL: mysql-5.7
* Redis: redis-3.2
* NodeJS: NodeJs-8.11.4
 
## Server Requirements
* CentOS Linux 或其他Linux 发行版
* docker >= 18.06.1-ce
* docker-compose >= 1.22.0

## 使用

1. 下载源码 
    
    Linux 服务器上自行选择位置下载源代码
    
    ```
    mkdir /var/www
    cd /var/www 
    git clone git@github.com:csthink/alpha-docker.git
    ```
    
2. 启动项目
    第一次会去 Docker Registry 下载镜像，国内的小伙伴记得配置 Docker 的国内镜像源，推荐阿里云，DaoCloud,网易
    
    ```
    cd /var/www/alpha-docker/docker # 进入 docker-compose.yml 文件的目录
    docker-compose up -d # 构建镜像，启动容器 -d 表示守护模式启动容器
    ```
3. 浏览器访问
    
    /var/www/docker/nginx/conf.d/alpha.conf 这里可以修改域名，这里选择默认不改
    * 配置好你本地的host 文件，将该域名指向 linux 服务器地址
    * 浏览器访问: http://www.alpha.com/ 
   
4. 关闭容器

    ```
    docker-compose down # 关闭容器
    ```
  
PS:
    Docker 容器的使用命令
    
    ```
    
    ```
