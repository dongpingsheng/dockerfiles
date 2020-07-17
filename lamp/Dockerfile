# NGINX + PHP with yaf + MySQL + SSH
# 
# Startup 
#
#    docker run -d -v /Users/dongping/dockerfiles/lamp:/data0  -p 3306:3306 -p 80:80 -p 1080:22 --name=lamp lamp 
#    docker run -it --rm -v /Users/dongping/dockerfiles/lamp:/data0  -p 3306:3306 -p 80:80 -p 1080:22 --name=lamp lamp /bin/bash
#
#    1) 此镜象直接把/data0目录设置为document_root目录，所以需要把你自己的web目录通过-v参数挂接到docker容器里。
#       测试方法：http://192.168.59.103/phpinfo.php
#    
#    2) ssh root@192.168.59.103 -p 1080   可以登录进容器 
#
#    3) Mysql配置：需要打开mysql权限
#       mysql -uroot -e "GRANT ALL ON *.* TO 'root'@'%' IDENTIFIED BY '';
#


# Pull base image.
FROM ubuntu:14.04
MAINTAINER dongping sheng <shengdongping@me.com>
 
RUN yum upgrade


# install build-essential 
RUN apt-get -y install build-essential

# install wget
RUN apt-get -y install wget

# install openssh-server
RUN apt-get -y install openssh-server

# install nginx
RUN yum install -y nginx

# install PHP
RUN apt-get -y install php5-cli php5-fpm php5-mysql
RUN apt-get -y install php5-dev
RUN apt-get -y install libpcre3-dev

# install mysql client
RUN apt-get -y install mysql-client-5.5

# Install MySQL.
RUN apt-get install -y mysql-server-5.5 

# install yaf framework
RUN wget http://pecl.php.net/get/yaf-2.2.9.tgz
RUN tar zxf yaf-2.2.9.tgz && cd yaf-2.2.9 && /usr/bin/phpize  && ./configure --with-php-config=/etc/php5/fpm/php.ini --with-php-config=/usr/bin/php-config5 && make && make install



################## CONFIG #####################

RUN mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak
COPY default /etc/nginx/sites-available/default

RUN sed -i s/\;cgi\.fix_pathinfo\s*\=\s*1/cgi.fix_pathinfo\=0/ /etc/php5/fpm/php.ini
RUN echo "extension=yaf.so" >> /etc/php5/fpm/php.ini

# 如果服务器内存不足，不建议用自定义my.cnf，用默认的即可
#COPY my.cnf /etc/mysql/my.cnf

# Config ssh server
RUN mkdir -p /var/run/sshd
RUN sed -i "s/^PermitRootLogin without-password/PermitRootLogin yes/g" /etc/ssh/sshd_config
RUN sed -i "s/^#GSSAPIAuthentication no/GSSAPIAuthentication no/g" /etc/ssh/sshd_config
RUN echo "UseDNS no" >> /etc/ssh/sshd_config
RUN echo 'root:root' | chpasswd

EXPOSE 3306
EXPOSE 22
EXPOSE 80

VOLUME ["/var", "/etc"]


# Define default command.
CMD service ssh start && service php5-fpm start && service mysql start && service nginx start && tail -f /var/log/nginx/access.log
