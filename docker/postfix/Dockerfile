FROM debian:jessie
ENV DEBIAN_FRONTEND noninteractive

ARG UID

RUN \
  useradd -u $UID www-aura && \
  apt-get update && apt-get install tzdata && \
  echo Europe/Kiev | tee /etc/timezone && \
  dpkg-reconfigure --frontend noninteractive tzdata && \
  apt-get install -y postfix rsyslog vim mutt sasl2-bin



RUN \
  openssl req -nodes -newkey rsa:2048 -keyout mail.domain.tld.key -out mail.domain.tld.csr  -subj '/C=UA/ST=Kharkov/L=Kharkov/CN=aurafit.com.ua' && \
  chmod 600 mail.domain.tld.key && \
  openssl x509 -req -days 365 -in mail.domain.tld.csr -signkey mail.domain.tld.key -out mail.domain.tld.crt && \
  openssl rsa -in mail.domain.tld.key -out mail.domain.tld.key.nopass && \
  mv mail.domain.tld.key.nopass mail.domain.tld.key && \
  openssl req -nodes -new -x509 -extensions v3_ca -keyout cakey.pem -out cacert.pem -days 365 -subj '/C=UA/ST=Kharkov/L=Kharkov/CN=aurafit.com.ua' && \
  chmod 600 mail.domain.tld.key && \
  chmod 600 cakey.pem && \
  mv mail.domain.tld.key /etc/ssl/private/ && \
  mv mail.domain.tld.crt /etc/ssl/certs/ && \
  mv cakey.pem /etc/ssl/private/ && \
  mv cacert.pem /etc/ssl/certs/


RUN \
  postconf -e "mydestination = localhost" && \
  postconf -e "myhostname = aurafit.com.ua" && \
  postconf -e "always_bcc = www-aura@localhost" && \
  postconf -e "smtpd_sasl_auth_enable=yes" && \
  postconf -e "broken_sasl_auth_clients=yes" && \
  postconf -e "smtpd_relay_restrictions=permit_sasl_authenticated, reject_unauth_destination" && \
  postconf -e "smtpd_sasl_security_options=noanonymous" && \
  postconf -e "smtpd_use_tls = yes" && \
  postconf -e 'smtpd_tls_key_file = /etc/ssl/private/mail.domain.tld.key' && \
  postconf -e 'smtpd_tls_cert_file = /etc/ssl/certs/mail.domain.tld.crt' && \
  postconf -e 'smtpd_tls_CAfile = /etc/ssl/certs/cacert.pem' && \
  postconf -e "smtpd_tls_loglevel = 1" && \
  postconf -e "smtpd_tls_received_header = yes" && \
  postconf -e "smtpd_tls_session_cache_timeout = 3600s" && \
  postconf -e "tls_random_source = dev:/dev/urandom" && \
  echo 123456 | saslpasswd2 -c -p -u aurafit.com.ua postfix && \
  ln  /etc/sasldb2 /var/spool/postfix/etc/sasldb2 && \
  adduser postfix sasl && \
  touch /var/log/mail.log


COPY ./smtpd.conf /etc/postfix/sasl/smtpd.conf

#postconf -e "smtpd_recipient_restrictions=permit_sasl_authenticated, reject_unauth_destination" && \
# postconf -e "smtpd_recipient_restrictions=permit_sasl_authenticated,reject_unauth_destination" && \
# postconf -e "smtpd_client_restrictions = permit_sasl_authenticated,reject_unauth_destination" && \
# postconf -F '*/*/chroot = y' && \
#  postconf -e "mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 $POSTFIX_MYNETWORKS" && \
#  postconf -e milter_default_action=accept && \
#  postconf -e milter_protocol=2 && \
#  postconf -e smtpd_milters=inet:dkim:8891 && \
#  postconf -e non_smtpd_milters=inet:dkim:8891 && \
#  LC_CTYPE=C.UTF-8    mutt -f  /var/mail/root

CMD service rsyslog start && service postfix start && tail -f /var/log/mail.log
