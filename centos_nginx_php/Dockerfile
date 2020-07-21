# NGINX + PHP with yaf
# 
# Startup 
#    
#    docker run -d -v /Users/dongping/dockerfiles/nginx\+php:/data0 -p 80:80 -p 1080:22 --name=nginx nginx 
#    
#    此镜象直接把/data0目录设置为document_root目录，所以需要把你自己的web目录通过-v参数挂接到docker容器里。
#    测试方法：http://192.168.59.103/phpinfo.php
#

FROM centos
MAINTAINER dongping sheng <shengdongping@me.com>

# update all 
RUN yum -y update
RUN yum -y upgrade


#base
RUN yum -y install wget vim


# install openssh-server
RUN yum -y install openssh-server


# install nginx
RUN yum -y install nginx

# install PHP
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
RUN yum -y install php php-cli php-common php-pdo php-json php-mysqlnd php-devel


RUN yum -y groupinstall "Development Tools"


# install yaf framework
RUN wget http://pecl.php.net/get/yaf-3.2.5.tgz
RUN tar zxf yaf-3.2.5.tgz && cd yaf-3.2.5 && /usr/bin/phpize  && ./configure --with-php-config=/etc/php.ini --with-php-config=/usr/bin/php-config && make && make install

RUN wget http://pecl.php.net/get/redis-5.3.1.tgz
RUN tar zxf redis-5.3.1.tgz && cd redis-5.3.1 && /usr/bin/phpize  && ./configure --with-php-config=/etc/php.ini --with-php-config=/usr/bin/php-config && make && make install


# config all
COPY redis.ini /etc/php.d/50-redis.ini
COPY yaf.ini /etc/php.d/50-yaf.ini
RUN sed -i s/\;cgi\.fix_pathinfo\s*\=\s*1/cgi.fix_pathinfo\=0/ /etc/php.ini


# Config ssh server
RUN mkdir -p /var/run/sshd
RUN sed -i "s/^PermitRootLogin without-password/PermitRootLogin yes/g" /etc/ssh/sshd_config
RUN sed -i "s/^#GSSAPIAuthentication no/GSSAPIAuthentication no/g" /etc/ssh/sshd_config
RUN echo "UseDNS no" >> /etc/ssh/sshd_config
RUN echo 'root:root' | chpasswd


EXPOSE 80

CMD service php7-fpm start && nginx

