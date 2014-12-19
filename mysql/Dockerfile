#
# MySQL Dockerfile
#
# Startup 
#
#    docker run -d -v /home/ubuntu/dockerfiles/mysql:/data0 -p 3306:3306 -v /var/lib/mysql/ --net="host" --name mysqlserver mysql
#
#    首先需要给当前mysqlserver添加各IP都能连进来的授权，否则mysqlserver只能在容器中访问。
#    所以在第一次启动时，需要进入bash配置一下，具体流程如下
#    1）docker run -it -v /home/ubuntu/dockerfiles/mysql:/data0 -p 3306:3306 -v /var/lib/mysql/ --name mysqlserver --net="host" --rm mysql /bin/bash
#    2) /usr/bin/mysqld_safe &
#    3) mysql -uroot -e "GRANT ALL ON *.* TO 'root'@'%' IDENTIFIED BY '';"
#    4) exit 
#

# Pull base image.
FROM ubuntu:14.04
MAINTAINER dongping sheng <shengdongping@me.com>

RUN apt-get update
RUN apt-get -y upgrade

# Install MySQL.
RUN apt-get install -y mysql-server 

COPY my.cnf /etc/mysql/my.cnf

EXPOSE 3306


# Define default command.
CMD ["/usr/bin/mysqld_safe"]

