FROM openresty/openresty:xenial
RUN apt-get update && apt-get  install -y supervisor cron logrotate
RUN mkdir -p /var/log/supervisor
COPY ./supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY ./logrotate.conf /etc/logrotate.conf
COPY ./cron.d /etc/cron.d/nginx
RUN chmod 644 /etc/logrotate.conf && chown root:root /etc/logrotate.conf
ARG UID
RUN addgroup --gid $UID --system app
RUN adduser --uid $UID --system --disabled-login --disabled-password --gid $UID app
ENTRYPOINT ["/usr/bin/supervisord"]
