# NGINX + PHP with yaf
# 
# Startup 
#
#    chcon -Rt svirt_sandbox_file_t /home/publish
#
#    docker run -v /home/publish/data0:/data0 -v /home/publish/log:/var/log -p 6379:6379 -p 80:80 -p 1080:22 -d --name=ubuntu_nginx_php_redis ubuntu_nginx_php_redis
#
#    docker run -v /home/publish/data0:/data0 -v /home/publish/log:/var/log -p 6379:6379 -p 80:80 -p 1080:22 -it ubuntu_nginx_php_redis /bin/bash
#    
#    此镜象直接把/data0目录设置为document_root目录，所以需要把你自己的web目录通过-v参数挂接到docker容器里。
#    测试方法：http://192.168.59.103/phpinfo.php
#

FROM ubuntu:14.04
MAINTAINER dongping sheng <shengdongping@me.com>

# update all 
RUN apt-get update
RUN apt-get -y upgrade

# install openssh-server
RUN apt-get -y install openssh-server


# install nginx

RUN apt-get -y install nginx

# install PHP
RUN apt-get -y install php php-cli php-fpm php-mysql

# install mysql client
RUN apt-get -y install mysql-client


# install build-essential
RUN apt-get -y install build-essential

# install yaf framework
RUN apt-get -y install php-dev
RUN apt-get -y install libpcre3-dev
RUN apt-get -y install wget
RUN wget http://pecl.php.net/get/yaf-3.2.5.tgz
RUN tar zxf yaf-3.2.5.tgz && cd yaf-3.2.5 && /usr/bin/phpize  && ./configure --with-php-config=/etc/php/7.4/fpm/php.ini --with-php-config=/usr/bin/php-config && make && make install

RUN wget http://pecl.php.net/get/redis-5.3.1.tgz
RUN tar zxf redis-5.3.1.tgz && cd redis-5.3.1 && /usr/bin/phpize  && ./configure --with-php-config=/etc/php/7.4/fpm/php.ini --with-php-config=/usr/bin/php-config && make && make install


# redis
RUN apt install -y redis-server


RUN apt install -y vim


# config all
RUN mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak
COPY redis.conf /etc/redis/redis.conf
COPY default /etc/nginx/sites-available/default
COPY php.ini /etc/php5/fpm/php.ini
RUN echo "extension=yaf.so" >> /etc/php/7.4/cli/php.ini
RUN echo "extension=redis.so" >> /etc/php/7.4/cli/php.ini
RUN echo "extension=yaf.so" >> /etc/php/7.4/fpm/php.ini
RUN echo "extension=redis.so" >> /etc/php/7.4/fpm/php.ini

# Config ssh server
RUN mkdir -p /var/run/sshd
RUN sed -i "s/^PermitRootLogin without-password/PermitRootLogin yes/g" /etc/ssh/sshd_config
RUN sed -i "s/^#GSSAPIAuthentication no/GSSAPIAuthentication no/g" /etc/ssh/sshd_config
RUN echo "UseDNS no" >> /etc/ssh/sshd_config
RUN echo 'root:root' | chpasswd


EXPOSE 80
EXPOSE 1080
EXPOSE 22
EXPOSE 6379

CMD service ssh start && service php7.4-fpm start && service nginx start && redis-server /etc/redis/redis.conf

