# phpIPAM docker container for Raspberri Pi 2/3
# https://phpipam.net
#
# References:
#  - Follow platform guideline specified in https://github.com/docker-library/official-images
# 

FROM arm32v7/ubuntu:17.04

LABEL maintainer "somsakc@hotmail.com"
LABEL version="1.0"
LABEL description="phpIPAM docker container for Raspberri Pi 2/3"

ARG PHPIPAM_VERSION=1.3

RUN apt-get update
#RUN apt-get install -y apache2 mariadb-server php php-pear php7.0-gmp php7.0-mysql php7.0-mbstring php7.0-gd php7.0-mcrypt php7.0-curl git
RUN apt-get install -y apache2 mariadb-client php php-pear php7.0-gmp php7.0-mysql php7.0-mbstring php7.0-gd php7.0-mcrypt php7.0-curl git
RUN apt-get install -y supervisor
RUN a2enmod rewrite

# Additional tools supporting phpIPAM
RUN apt-get install -y inetutils-ping

RUN mkdir /config

COPY apache2.conf /config/apache2.conf
RUN cat /config/apache2.conf >> /etc/apache2/apache2.conf && \
    rm -f /config/apache2.conf

RUN mv /var/www/html /var/www/html.old && \
    mkdir /var/www/html && \
    cd /var/www/html && \
    git clone https://github.com/phpipam/phpipam.git . && \
    git checkout $PHPIPAM_VERSION && \
    rmdir /var/www/html/functions/PHPMailer

RUN cd /tmp && \
    git clone https://github.com/PHPMailer/PHPMailer.git && \
    mv PHPMailer /var/www/html/functions

RUN chown www-data:www-data -R /var/www/html && \
    cd /var/www/html && \
    find . -type f -exec chmod 0644 {} \; && \
    find . -type d -exec chmod 0755 {} \;

COPY phpipam-init /config/phpipam-init
RUN chmod +x /config/phpipam-init

# configure container interfaces
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
CMD ["/usr/bin/supervisord"]

EXPOSE 80/tcp

#VOLUME ["/opt/observium/lock", "/opt/observium/logs","/opt/observium/rrd"]

