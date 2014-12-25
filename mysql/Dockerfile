#
# MySQL Dockerfile
#
# Startup 
#
#    docker run -d -v /var/ -v /usr/ -v /etc -p 3306:3306 -p 1081:22 --name mysqlserver mysql
#
#    首先需要给当前mysqlserver添加各IP都能连进来的授权，否则mysqlserver只能在容器中访问。
#    所以在第一次启动时，需要进入bash配置一下，具体流程如下
#    mysql -uroot -e "GRANT ALL ON *.* TO 'root'@'%' IDENTIFIED BY '';
#

# Pull base image.
FROM ubuntu:14.04
MAINTAINER dongping sheng <shengdongping@me.com>

RUN apt-get update
RUN apt-get -y upgrade

# install openssh-server
RUN apt-get -y install openssh-server

# Install MySQL.
RUN apt-get install -y mysql-server 

COPY my.cnf /etc/mysql/my.cnf

# Config ssh server
RUN mkdir -p /var/run/sshd
RUN sed -i "s/^PermitRootLogin without-password/PermitRootLogin yes/g" /etc/ssh/sshd_config
RUN sed -i "s/^#GSSAPIAuthentication no/GSSAPIAuthentication no/g" /etc/ssh/sshd_config
RUN echo "UseDNS no" >> /etc/ssh/sshd_config
RUN echo 'root:root' | chpasswd

EXPOSE 3306
EXPOSE 1081
EXPOSE 22


# Define default command.
CMD service ssh start && /usr/bin/mysqld_safe

