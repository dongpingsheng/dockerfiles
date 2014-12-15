#
# MySQL Dockerfile
#
# Startup 
#    docker run -it -v /Users/dongping/mysqldata:/data0 -p 3306 mysql
#

# Pull base image.
FROM ubuntu
MAINTAINER dongping sheng <shengdongping@me.com>

# RUN apt-get update
# RUN apt-get -y upgrade

# Install MySQL.
RUN apt-get install -y mysql-server 

# 停止MYSQL
RUN service mysql stop
COPY my.cnf /etc/mysql/my.cnf

# Expose ports.
EXPOSE 3306


# Define default command.
CMD ["service", "mysql", "start"]

