FROM ubuntu:xenial

ENV DEBIAN_FRONTEND noninteractive
ENV LANG C.UTF-8

RUN apt-get update && \
    apt-get install -y software-properties-common && \
    add-apt-repository ppa:ondrej/php && \
    apt-get update && \
    apt-get install -y \
    php5.6 \
    php5.6-cgi \
    php5.6-cli \
    php5.6-common \
    php5.6-fpm \
    php5.6-mysql \
    php5.6-mbstring \
    php5.6-curl \
    php5.6-dev \
    php5.6-gd \
    php5.6-xml \
    php5.6-mcrypt \
    php5.6-xmlrpc \
    php5.6-zip \
    php5.6-soap \
    php5.6-json \
    vim \
    cron \
    php5.6-mysqli

ARG UID

RUN useradd  -u $UID app \
 && mkdir -p /run && mkdir -p  /run/php/

CMD ["/usr/sbin/php-fpm5.6", "--nodaemonize"]
