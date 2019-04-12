FROM ubuntu:latest
MAINTAINER Mischa Wolf

# Install Apache
RUN apt-get update
RUN apt-get -q -y install apache2 

# Configure Apache
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2

RUN mkdir -p /var/lock/apache2 /var/run/apache2 \
&& rm /var/www/html/index.html

# Expose Port 80
EXPOSE 80

CMD /bin/bash -c "source /etc/apache2/envvars && exec /usr/sbin/apache2 -DFOREGROUND"