# dnmp
docker搭建lnmp环境，php 7.2 + nginx latest + mysql 5.7 + redis 4

## 前言
  使用前，默认你已经安装了 docker 和 docker-compose

## 先使用阿里云镜像加速器
 
 [阿里云镜像加速器](https://dev.aliyun.com)
 
 进入之后登录，然后进到 [ 管理中心 ]，找到镜像加速器，里面有教你怎么配置本地的加速器，不再赘述
  
 ##  克隆此包
  
     git clone  https://github.com/chichoyi/dnmp.git

### 修改docker-compose.yml

 这里会说明一些参数
 
    nginx:
      image: nginx:latest
      container_name: nginx
      hostname: nginx
      ports:
        - "80:80"
      volumes:
        - /data/www:/www  # /data/www 这个是宿主机的目录，可以修改指向你的代码文件夹，比如 /mnt/hgfs/www
        - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro  # 这里是配置文件，不多，可以打开看看
        - ./nginx/conf.d/:/etc/nginx/conf.d/:ro
      links:
        - php:PHP-FPM  # php容器的别名，nginx发送给php需要用到的，默认即可
    
    php:
      image: php:7.2.2-fpm  # 镜像版本，可以自己指定，只要镜像仓库拉得到就行
      container_name: php
      hostname: php
      ports:
        - "9000：9000"
      links:
        - mysql:mysql
        - redis:redis
      volumes:
        - /data/www:/www  # /data/www 这个是宿主机的目录，可以修改指向你的代码文件夹，比如 /mnt/hgfs/www
        
        # 注意，nginx和php挂载出来的宿主机目录一定要一致(/data/www)，不然没法访问php文件
    
    mysql:
       image: mysql:5.7
       container_name: mysql
       hostname: mysql
       ports:
         - "3306:3306"
       environment:
        - MYSQL_ROOT_PASSWORD=root  #数据库的初始化密码，这里可以指定修改成你自己的
    
    redis:
      image: redis:4
      container_name: redis
      hostname: redis
      ports:
        - "6379:6379"
   
   
 ## 修改完之后执行命令
  
    cd dnmp
    
    docker-compose up -d
    
 第一次执行需要花点时间下载镜像
 
 ## tip
 
   使用的都是官方的镜像，后面有时间再自己构建轻量级的docker镜像，如有疑问，欢迎提出指正
   
