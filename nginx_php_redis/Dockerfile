# NGINX + PHP with yaf
# 
# Startup 
#    
#    docker run -d -v /Users/dongping/dockerfiles/nginx\+php:/data0 -p 80:80 -p 1080:22 --name=nginx nginx 
#
#    docker run -it -v /home/data0:/data0  -p 3306:3306 -p 80:80 -p 22:22 --name=lamp lamp /bin/bash
#    
#    此镜象直接把/data0目录设置为document_root目录，所以需要把你自己的web目录通过-v参数挂接到docker容器里。
#    测试方法：http://192.168.59.103/phpinfo.php
#

FROM ubuntu:latest
MAINTAINER dongping sheng <shengdongping@me.com>

# update all 
RUN apt-get update
RUN apt-get -y upgrade


# install nginx

RUN apt-get -y install nginx
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak


# install PHP
RUN apt install -y php7.4 php7.4-cli php7.4-fpm php7.4-mysql php7.4-json php7.4-bcmath php7.4-common php7.4-curl php-redis
RUN sed -i s/\;cgi\.fix_pathinfo\s*\=\s*1/cgi.fix_pathinfo\=0/ /etc/php5/fpm/php.ini

# install mysql client
RUN apt-get -y install mysql-client


COPY default /etc/nginx/sites-available/default

EXPOSE 80 

CMD service php7-fpm start && nginx 

