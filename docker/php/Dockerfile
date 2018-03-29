FROM php:5.6-fpm
ARG UID
RUN addgroup --gid $UID --system app
RUN adduser --uid $UID --system --disabled-login --disabled-password --gid $UID app
