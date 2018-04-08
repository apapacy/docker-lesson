FROM debian:jessie
ENV DEBIAN_FRONTEND noninteractive

ARG UID

RUN \
  useradd -u $UID www-aura && \
  apt-get update && apt-get install tzdata && \
  echo Europe/Kiev | tee /etc/timezone && \
  dpkg-reconfigure --frontend noninteractive tzdata && \
  apt-get install -y postfix rsyslog vim mutt sasl2-bin && \
  postconf -e "mydestination = localhost" && \
  postconf -e "myhostname = aurafit.com.ua" && \
  postconf -e "always_bcc = www-aura@localhost" && \
  postconf -e "smtpd_sasl_auth_enable=yes" && \
  postconf -e "broken_sasl_auth_clients=yes" && \
  postconf -e "smtpd_recipient_restrictions=permit_sasl_authenticated,reject_unauth_destination" && \
  postconf -e "smtpd_client_restrictions = permit_sasl_authenticated,reject_unauth_destination" && \
  postconf -e "smtpd_sasl_security_options = noanonymous" && \
  echo 123456 | saslpasswd2 -c -p -u aurafit.com.ua postfix && \
  ln  /etc/sasldb2 /var/spool/postfix/etc/sasldb2 && \
  adduser postfix sasl && \
  touch /var/log/mail.log


COPY ./smtpd.conf /etc/postfix/sasl/smtpd.conf

# postconf -F '*/*/chroot = y' && \
#  postconf -e "mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 $POSTFIX_MYNETWORKS" && \
#  postconf -e milter_default_action=accept && \
#  postconf -e milter_protocol=2 && \
#  postconf -e smtpd_milters=inet:dkim:8891 && \
#  postconf -e non_smtpd_milters=inet:dkim:8891 && \
#  LC_CTYPE=C.UTF-8    mutt -f  /var/mail/root

CMD service rsyslog start && service postfix start && tail -f /var/log/mail.log
