FROM openresty/openresty:xenial
RUN apt-get update && apt-get  install -y supervisor cron
RUN mkdir -p /var/log/supervisor
COPY ./supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ARG UID
RUN addgroup --gid $UID --system app
RUN adduser --uid $UID --system --disabled-login --disabled-password --gid $UID app
ENTRYPOINT ["/usr/bin/supervisord"]
# CMD ["-c", "/etc/supervisor/conf.d/supervisord.conf"]
