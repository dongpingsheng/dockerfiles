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
RUN apt update
RUN apt -y upgrade

# base
RUN apt install -y vim 
RUN apt install -y wget
# install build-essential
RUN apt -y install build-essential


# install nginx
RUN apt install -y nginx
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak
COPY default /etc/nginx/sites-available/default


# install PHP
RUN apt install -y php7.4 php7.4-cli php7.4-fpm php7.4-mysql php7.4-json php7.4-bcmath php7.4-common php7.4-curl php-redis
COPY php.ini /etc/php/7.4/fpm/php.ini

# install yaf framework
RUN apt -y install php7.4-dev libpcre3-dev
RUN wget https://github.com/laruence/yaf/archive/yaf-3.2.5.tar.gz
RUN tar zxf yaf-3.2.5.tar.gz && cd yaf-yaf-3.2.5/
RUN /usr/bin/phpize  && ./configure --with-php-config=/etc/php7.4/fpm/php.ini --with-php-config=/usr/bin/php-config && make && make install
RUN echo "extension=yaf.so" >> /etc/php/7.4/fpm/php.ini


# install mysql client
RUN apt -y install mysql-client


EXPOSE 80 

CMD service php7-fpm start && nginx 

