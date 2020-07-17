# NGINX + PHP with yaf
#
# Startup
#
#    chcon -Rt svirt_sandbox_file_t /home/publish
#
#    docker run -d -v /home/publish/data0:/data0 -v /home/publish/log:/var/log -p 80:80 -p 6379:6379 --name=ubuntu_redis ubuntu_redis
#
#    docker run -it -v /home/publish/data0:/data0 -v /home/publish/log:/var/log -p 80:80 -p 6379:6379 ubuntu_redis /bin/bash
#
#    此镜象直接把/data0目录设置为document_root目录，所以需要把你自己的web目录通过-v参数挂接到docker容器里。
#    测试方法：http://192.168.59.103/phpinfo.php
#

FROM ubuntu:latest
MAINTAINER dongping sheng <shengdongping@me.com>

# update all
RUN apt-get update --fix-missing
RUN apt-get -y upgrade

# base
RUN apt install -y apt-utils

# redis
RUN apt install redis-server redis

EXPOSE 6379

CMD redis-server