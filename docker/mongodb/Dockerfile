FROM mongo
ARG UID
RUN adduser --uid $UID --system --disabled-login --disabled-password -ingroup mongodb app
USER app
