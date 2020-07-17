# NGINX + PHP with yaf
# 
# Startup 
#    
#    docker run -v /home/publish/data0:/data0 -v /home/publish/log:/var/log -p 80:80 -p 1080:22 -it ubuntu_nginx_php /bin/bash
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
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak


# install PHP
RUN apt-get -y install php5-cli php5-fpm php5-mysql

# install mysql client
RUN apt-get -y install mysql-client-5.5


# install build-essential 
RUN apt-get -y install build-essential

# install yaf framework
RUN apt-get -y install php5-dev
RUN apt-get -y install libpcre3-dev
RUN apt-get -y install wget
RUN wget http://pecl.php.net/get/yaf-2.2.9.tgz
RUN tar zxf yaf-2.2.9.tgz && cd yaf-2.2.9 && /usr/bin/phpize  && ./configure --with-php-config=/etc/php5/fpm/php.ini --with-php-config=/usr/bin/php-config5 && make && make install


# Config ssh server
RUN mkdir -p /var/run/sshd
RUN sed -i "s/^PermitRootLogin without-password/PermitRootLogin yes/g" /etc/ssh/sshd_config
RUN sed -i "s/^#GSSAPIAuthentication no/GSSAPIAuthentication no/g" /etc/ssh/sshd_config
RUN echo "UseDNS no" >> /etc/ssh/sshd_config
RUN echo 'root:root' | chpasswd

COPY default /etc/nginx/sites-available/default
COPY php.ini /etc/php5/fpm/php.ini
RUN echo "extension=yaf.so" >> /etc/php5/fpm/php.ini

EXPOSE 80 
EXPOSE 1080 
EXPOSE 22

CMD service ssh start && service php5-fpm start && nginx 

