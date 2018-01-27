FROM openresty/openresty:xenial
RUN apt-get update && apt-get  install -y supervisor cron logrotate
COPY ./supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY ./logrotate.conf /etc/logrotate.conf
COPY ./cron.d /etc/cron.d/nginx
ARG UID
RUN mkdir -p /var/log/supervisor \
  && chmod 644 /etc/logrotate.conf && chown root:root /etc/logrotate.conf \
  && chmod 644 /etc/cron.d/nginx && chown root:root /etc/cron.d/nginx \
  && addgroup --gid $UID --system app \
  && adduser --uid $UID --system --disabled-login --disabled-password --gid $UID app
ENTRYPOINT ["/usr/bin/supervisord"]
