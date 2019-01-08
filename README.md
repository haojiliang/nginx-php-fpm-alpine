[![Docker Pulls](https://img.shields.io/docker/pulls/haojiliang/nginx-php-fpm-alpine.svg)](https://hub.docker.com/r/haojiliang/nginx-php-fpm-alpine)
[![Docker Stars](https://img.shields.io/docker/stars/haojiliang/nginx-php-fpm-alpine.svg)](https://hub.docker.com/r/haojiliang/nginx-php-fpm-alpine)
  
# nginx-php-fpm-alpine  
docker pull haojiliang/nginx-php-fpm-alpine:v1.15.7  
  
nginx 1.15.7    
php 7.0.33  
php-fpm  
alpine 3.7  
  
# php default.conf
```
location ~ \.php$ {
    root           html;
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
```
  
# docker-compose.yml  
```
version: '3.1'

services:

  web:
    image: haojiliang/nginx-php-fpm-alpine:v1.15.7
    container_name: "web_container"
    ports:
      - "80:80"
      - "443:443"
    extra_hosts:
      - "www.vhxsl.com:127.0.0.1"
      - "m.vhxsl.com:127.0.0.1"
    volumes:
      - /etc/localtime:/etc/localtime
      - /etc/timezone:/etc/timezone
      - /data/nginx/html/wordpress:/data/www/wordpress
      - /data/nginx/html/default:/usr/share/nginx/html
      - /data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf
      - /data/nginx/conf/conf.d:/etc/nginx/conf.d
      - /data/nginx/conf/cert:/etc/nginx/cert
      - /data/nginx/logs:/var/log/nginx
    depends_on:
      - mysql
    restart: always

  mysql:
    image: mysql:5.7.24
    container_name: "mysql_container"
    volumes:
      - /etc/localtime:/etc/localtime
      - /etc/timezone:/etc/timezone
      - /data/mysql/data:/var/lib/mysql
      - /data/mysql/conf:/etc/mysql/conf.d
      - /data/mysql/logs:/data/mysql/logs
    restart: always
    ports:
      - "3306:3306"
```