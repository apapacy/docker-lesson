FROM openresty/openresty:xenial
RUN apt-get update && apt-get  install -y supervisor cron logrotate
RUN openssl dhparam -dsaparam -out /etc/ssl/certs/dhparam.pem 4096

COPY ./supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY ./logrotate.conf /etc/logrotate.conf
COPY ./cron.d /etc/cron.d/nginx

ARG UID

RUN mkdir -p /var/log/supervisor \
  && chmod 644 /etc/logrotate.conf && chown root:root /etc/logrotate.conf \
  && chmod 644 /etc/cron.d/nginx && chown root:root /etc/cron.d/nginx \
  && useradd -u $UID app

ENTRYPOINT ["/usr/bin/supervisord"]
