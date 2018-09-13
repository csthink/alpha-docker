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
    
    docker-compose 构建项目 
    
    ```
    cd /var/www/alpha-docker/docker # 进入 docker-compose.yml 文件的目录
    alpha 是一个Laravel 项目，记得创建 .env 文件，具体可以参考 .env.example,上传本地项目到服务器时，记得将 .env 文件准备好上传上去
    docker-compose up -d # 构建镜像，启动容器 -d 表示守护模式启动容器
    docker-compose down # 关闭容器并删除服务
    ```
    
    使用 Composer
    alpha 项目依赖 Composer 进行构建,我们在创建 php-fpm 容器时就已经将 Composer 安装在容器中
    使用 composer 有两种方式，
    方式1：用 docker-compose 操作(推荐)
    
    ```
    docker-compose run --rm -w /data/www/alpha php-fpm composer install
    # -w /data/www/alpha 是 php-fpm 的工作区域，alpha 项目也是挂载在里面
    ```
    
    方式2：进入宿主机（容器外部）app 目录下用 docker 命令
    
    ```
    cd alpah-docker/app
    docker run -it --rm -v `pwd`:/data/www/ -w /data/www/alpha docker_php-fpm composer install
    ```
     
3. 浏览器访问
    
    /var/www/docker/nginx/conf.d/alpha.conf 这里可以修改域名，这里选择默认不改
    * 配置好你本地的host 文件，将该域名指向 linux 服务器地址
    * 浏览器访问: http://www.alpha.com/  