# NGINX + PHP with yaf
# 
# Startup 
#    docker run -d -v /Users/dongping/dockerfiles/nginx\+php:/data0 -p 80:80 nginx 
#

FROM ubuntu
MAINTAINER dongping sheng <shengdongping@me.com>

# update all 
RUN apt-get update
RUN apt-get -y upgrade

# install build-essential 
RUN apt-get -y install build-essential


# install nginx

RUN apt-get -y install nginx
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak
COPY default /etc/nginx/sites-available/default

# install PHP
RUN apt-get -y install php5-cli php5-fpm php5-mysql
RUN sed -i s/\;cgi\.fix_pathinfo\s*\=\s*1/cgi.fix_pathinfo\=0/ /etc/php5/fpm/php.ini


# install yaf framework
RUN apt-get -y install php-dev
RUN apt-get -y install libpcre3-dev
RUN wget http://pecl.php.net/get/yaf-2.2.9.tgz
RUN cd yaf-2.2.9
RUN phpize 
RUN ./configure --with-php-config=/etc/php5/fpm/php.ini --with-php-config=/usr/bin/php-config5 
RUN make 
RUN make install

# uninstall build-essential
RUN apt-get 

EXPOSE 80 

CMD service php5-fpm start && nginx 

