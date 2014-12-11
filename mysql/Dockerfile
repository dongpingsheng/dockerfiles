#
# MySQL Dockerfile
#
#

# Pull base image.
FROM ubuntu
MAINTAINER dongping sheng <shengdongping@me.com>

# Install MySQL.
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install -y mysql-server  
COPY my.cnf /etc/mysql/my.cnf

# Define working directory.
WORKDIR /data1

# Expose ports.
EXPOSE 3306


# Define default command.
CMD ["mysqld"]

