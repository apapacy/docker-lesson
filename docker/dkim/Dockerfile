FROM debian:jessie
ENV DEBIAN_FRONTEND noninteractive

# Выставляем временную зону
RUN echo Europe/Kiev | tee /etc/timezone
RUN dpkg-reconfigure --frontend noninteractive tzdata

ARG DKIM_DOMAIN
ARG DKIM_SELECTOR
ARG DKIM_INTERNAL

# Устанавливаем зависимости и rsyslog для логов. apt-get update намеренно находится вместе с install для предотвращение использования кэша

RUN apt-get update && apt-get install -y opendkim opendkim-tools openssl rsyslog

# Создаем ключи и добавляем конфигурацию OpenDKIM, создаем лог файл. Также выводим на экран открытый ключ для добавления в TXT запись нашего домена.

RUN \
 mkdir /etc/opendkim/ && \
 opendkim-genkey -D /etc/opendkim/ -d $DKIM_DOMAIN -s $DKIM_SELECTOR && \
 cat /etc/opendkim/$DKIM_SELECTOR.txt && \
 printf "\
KeyTable file:/etc/opendkim/keytable \n\
SigningTable file:/etc/opendkim/signingtable \n\
InternalHosts file:/etc/opendkim/internal \n\
Canonicalization relaxed/relaxed \n\
LogWhy yes \n\
X-Header yes \n\
SyslogSuccess yes \n" >> /etc/opendkim.conf && \
 printf "$DKIM_INTERNAL\n" >> /etc/opendkim/internal && \
 printf "$DKIM_SELECTOR._domainkey.$DKIM_DOMAIN $DKIM_DOMAIN:$DKIM_SELECTOR:/etc/opendkim/$DKIM_SELECTOR.private\n" >> /etc/opendkim/keytable && \
 printf "* mail._domainkey.$DKIM_DOMAIN\n" >> /etc/opendkim/signingtable && \
 printf "SOCKET=\"inet:8891@0.0.0.0\"\n" >> /etc/default/opendkim && \
 touch /var/log/mail.log

# Разрешаем читать ключ только владельцу

RUN chown opendkim /etc/opendkim/$DKIM_SELECTOR.private && \
 chmod 600 /etc/opendkim/$DKIM_SELECTOR.private

# Команда для старта сервиса. OpenDKIM использует rsyslog для выдачи логов, поэтому запускаем и его
CMD service rsyslog start && service opendkim start && tail -f /var/log/mail.log
