#
# MySQL Dockerfile
#
# Startup 
#    docker run -it -v /Users/dongping/mysqldata:/data0 -p 3306:3306 --name mysqlserver mysql
#

# Pull base image.
FROM ubuntu
MAINTAINER dongping sheng <shengdongping@me.com>

# RUN apt-get update
# RUN apt-get -y upgrade

# Install MySQL.
RUN apt-get install -y mysql-server 

COPY my.cnf /etc/mysql/my.cnf

RUN echo "GRANT ALL ON *.* TO 'root'@'%' IDENTIFIED BY '';" >/etc/mysql/init
EXPOSE 3306


# Define default command.
CMD ["/usr/bin/mysqld_safe"]

