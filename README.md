## 项目说明
* docker 学习笔记

* 建议每个目录都单独一个 README.md 文件


## 学习参考
https://www.bilibili.com/video/BV11L411g7U1/

https://docker.easydoc.net/doc/81170005/cCewZWoN/lTKfePfP

https://docs.docker.com/engine/reference/commandline/run/



## df -h
```sh
Filesystem      Size  Used Avail Use% Mounted on
udev            1.9G     0  1.9G   0% /dev
tmpfs           378M  1.4M  376M   1% /run
/dev/sda1       110G  9.2G   95G   9% /
tmpfs           1.9G   25M  1.9G   2% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           1.9G     0  1.9G   0% /sys/fs/cgroup
tmpfs           378M   24K  378M   1% /run/user/1000
/dev/loop0       90M   90M     0 100% /snap/core/8268
/dev/loop1       68M   68M     0 100% /snap/sublime-text/85
```



## nginx
* https://hub.docker.com/_/nginx
* https://hub.docker.com/_/nginx?tab=reviews

```sh
docker pull nginx:1.17

docker run --name lamp-nginx -p 8000:80 -v /home/u/DockerTest/lamp/www:/usr/share/nginx/html -d nginx:1.17 

docker exec -it lamp-nginx bash

docker rm lamp-nginx
```

* 完整 nginx 案例：
```sh
docker run --name lamp-nginx \
	-p 8000:80 \
	-v /home/u/DockerTest/lamp/www:/usr/share/nginx/html \
	-v /home/u/DockerTest/lamp/nginx/nginx.conf:/etc/nginx/nginx.conf:ro \
	-v /home/u/DockerTest/lamp/nginx/logs:/var/log/nginx \
	-v /home/u/DockerTest/lamp/nginx/conf.d:/var/log/conf.d \
#	-v /home/u/DockerTest/lamp/nginx/conf.d:/etc/nginx/conf.d 
	# 时区
	-v /etc/localtime:/etc/localtime \
	# debug 可不开启（尚不知道开启的作用）
	-d nginx:1.17 nginx-debug -g 'daemon off;'


# 修改nginx配置后重启
docker restart lamp-nginx
# 查看错误
docker logs lamp-nginx
```



## mysql
* https://hub.docker.com/_/mysql

* 完整 mysql 案例：
```sh
docker run --name lamp-mysql \
	-p 33060:3306 \
	-v /home/u/DockerTest/lamp/mysql/conf.d:/etc/mysql/conf.d \
	-v /home/u/DockerTest/lamp/mysql/logs:/var/log/mysql \
	-v /home/u/DockerTest/lamp/mysql/data:/var/lib/mysql \
	# 时区
	-v /etc/localtime:/etc/localtime \
	-e MYSQL_ROOT_PASSWORD=mysql111111 \
	-d mysql:5.7 \
	--character-set-server=utf8mb4 \
	--collation-server=utf8mb4_unicode_ci
```



## php
* https://hub.docker.com/_/php

```sh
docker pull php:7.3-fpm

docker run --name lamp-php -v /home/u/DockerTest/lamp/www:/var/www/html -d php:7.3-fpm 

docker exec -it lamp-php bash // 进入 lamp-php bash 后可按装 php 扩展如下
	* docker-php-ext-install pdo pdo_mysql;
	* apt update; apt install libpng-dev; docker-php-ext-install dg;
	* php -m // 查看已安装扩展 
	* pecl install redis && docker-php-ext-enable redis
	* 编译安装 php-redis扩展: https://www.cnblogs.com/wyaokai/p/11904701.html 

```



## redis
* https://hub.docker.com/_/redis

```sh
docker run --name lamp-redis \
	-p 63790:6379 \
	-v /home/u/DockerTest/lamp/redis/redis.conf:/usr/local/etc/redis/redis.conf \
	-v /home/u/DockerTest/lamp/redis/data:/data \
	-v /etc/localtime:/etc/localtime \
	-d redis:5 \
	redis-server /usr/local/etc/redis/redis.conf --appendonly yes
```
