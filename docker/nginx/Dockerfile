FROM openresty/openresty
ARG UID
RUN addgroup -g $UID -S app
RUN adduser -u $UID -S -D -H -G app app
