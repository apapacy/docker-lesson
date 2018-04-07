FROM debian:jessie
ENV DEBIAN_FRONTEND noninteractive

RUN echo Europe/Kiev | tee /etc/timezone
RUN dpkg-reconfigure --frontend noninteractive tzdata

ARG POSTFIX_MYNETWORKS

RUN apt-get update && apt-get install -y postfix rsyslog vim mutt sasl2-bin

# Конфигурация postfix для обработки писем через opendkim и создание лог-файла

ARG UID

RUN \
 postconf -e "mydestination = localhost" && \
 postconf -e "mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 $POSTFIX_MYNETWORKS 0.0.0.0" && \
 postconf -e "myhostname = aurafit.com.ua" && \
 postconf -e "relay_domains = meta.ua" && \
 postconf -e "always_bcc = www-aura@localhost" && \
 useradd -u $UID www-aura && \
 touch /var/log/mail.log && \
 echo postfix | saslpasswd2 -c -p postfix

# RUN \
#  postconf -e "mydestination = localhost" && \
#  postconf -e "mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 $POSTFIX_MYNETWORKS" && \
#  postconf -e milter_default_action=accept && \
#  postconf -e milter_protocol=2 && \
#  postconf -e smtpd_milters=inet:dkim:8891 && \
#  postconf -e non_smtpd_milters=inet:dkim:8891 && \
#  touch /var/log/mail.
#  LC_CTYPE=C.UTF-8    mutt -f  /var/mail/root

#postconf -e "smtpd_sasl_auth_enable = yes" && \
#postconf -e "smtpd_sasl_security_options = noanonymous" && \
#postconf -e "smtpd_client_restrictions = permit_sasl_authenticated" && \
#postconf -e "smtpd_sender_restrictions = permit_sasl_authenticated" && \
#postconf -e "smtpd_recipient_restrictions = permit_mynetworks, permit_sasl_authenticated, permit_auth_destination, reject_unauth_destination" && \
#postconf -e "broken_sasl_auth_clients = no" && \



CMD service rsyslog start && service postfix start && tail -f /var/log/mail.log
