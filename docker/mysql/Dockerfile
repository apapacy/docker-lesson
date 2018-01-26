FROM mysql:5.7
ARG UID
RUN adduser --uid $UID --system --disabled-login --disabled-password -ingroup mysql app
USER app
