# nginx_php-fpm_mysql_redis
dockerfiles that support app's working environment

## 简介
用 Docker 容器服务的方式搭建 php应用 环境，易于维护、升级。使用前需了解 Docker 的基本概念，常用基本命令。
可以一条条命令执行 docker 命令来构建镜像，容器。这里推荐使用 docker-compose 来管理，执行项目，下面是使用流程。

相关软件版本：
- PHP 7.2
- MySQL 5.7
- Nginx 1.12
- Redis 3.2

用到的 PHP 拓展(2018.2.9更新)：
- redis 3.1.4

## 使用
### 1.安装 Docker，Docker-compose  
- Docker，详见官方文档：https://docs.docker.com/engine/installation/linux/docker-ce/centos/
- docker-compose，文档：https://docs.docker.com/compose/install/
```
sudo pip install -U docker-compose
```
### 2.docker-compose 构建项目
进入 docker-compose.yml 所在目录：
执行命令：
```
docker-compose up
```  

如果没问题，下次启动时可以以守护模式启用，所有容器将后台运行：  
```
docker-compose up -d
``` 

使用 docker-compose 基本上就这么简单，Docker 就跑起来了，用 stop，start 关闭开启容器服务。  
更多的是在于编写 dockerfile 和 docker-compose.yml 文件。 

可以这样关闭容器并删除服务：
```
docker-compose down
```

### 3. 使用 Composer
php应用项目可能依赖 Composer 进行构建。

我们在创建 PHP-fpm 容器时就已经将 Composer 安装在容器中，可以运行该容器进行 Composer 操作。

用 docker-compose 进行操作：
```
docker-compose run --rm -w /data/www/demo_app php-fpm composer update
```
`-w /data/www/demo_app`为在php-fpm的工作区域，demo_app项目也是挂载在里面，所有我们可以直接在容器里运行composer。

或者进入宿主机（容器外部）app 目录下用 docker 命令：
```
cd nginx_php-fpm_mysql_redis/app

docker run -it --rm -v `pwd`:/data/www/ -w /data/www/demo_app files_php-fpm composer update
```
