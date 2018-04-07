FROM debian:jessie
ENV DEBIAN_FRONTEND noninteractive

RUN echo Europe/Kiev | tee /etc/timezone
RUN dpkg-reconfigure --frontend noninteractive tzdata

ARG POSTFIX_MYNETWORKS

RUN apt-get update && apt-get install -y postfix rsyslog vim mutt

# Конфигурация postfix для обработки писем через opendkim и создание лог-файла

RUN \
 postconf -e "mydestination = localhost" && \
 postconf -e "mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 $POSTFIX_MYNETWORKS 0.0.0.0" && \
 postconf -e "myhostname = localhost" && \
 postconf -e "relay_domains = auafit.com.ua" && \
 postconf -e "always_bcc = root@localhost" && \
 touch /var/log/mail.log && \
 touch /var/mail/root && \
 chmod 777 /var/mail/root

# RUN \
#  postconf -e "mydestination = localhost" && \
#  postconf -e "mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 $POSTFIX_MYNETWORKS" && \
#  postconf -e milter_default_action=accept && \
#  postconf -e milter_protocol=2 && \
#  postconf -e smtpd_milters=inet:dkim:8891 && \
#  postconf -e non_smtpd_milters=inet:dkim:8891 && \
#  touch /var/log/mail.
#  LC_CTYPE=C.UTF-8    mutt -f  /var/mail/root

CMD service rsyslog start && service postfix start && tail -f /var/log/mail.log
