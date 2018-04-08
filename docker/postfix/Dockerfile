FROM ubuntu:xenial
# ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install tzdata
RUN echo Europe/Kiev | tee /etc/timezone
RUN dpkg-reconfigure --frontend noninteractive tzdata

ARG UID

RUN apt-get install -y postfix rsyslog vim mutt sasl2-bin

RUN \
 postconf -F '*/*/chroot = n' && \
 postconf -e "mydestination = localhost" && \
 postconf -e "myhostname = aurafit.com.ua" && \
 postconf -e "always_bcc = www-aura@localhost" && \
 postconf -e "smtpd_sasl_auth_enable=yes" && \
 postconf -e "broken_sasl_auth_clients=yes" && \
 postconf -e "smtpd_recipient_restrictions=permit_sasl_authenticated,reject_unauth_destination" && \
 postconf -e "smtpd_client_restrictions = permit_sasl_authenticated,reject_unauth_destination" && \
 postconf -e "smtpd_sasl_security_options = noanonymous" && \
 echo 123456 | saslpasswd2 -c -p -u aurafit.com.ua -a smtpd000 postfix && \
 useradd -u $UID www-aura && \
 adduser postfix sasl && \
 touch /var/log/mail.log


# postconf -e "mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128" && \
# echo 123456 | saslpasswd2 -c -p -u aurafit.com.ua  postfix && \
COPY ./smtpd.conf /etc/postfix/sasl/smtpd.conf



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
#postconf -e "smtpd_sender_restrictions = permit_sasl_authenticated" && \
#postconf -e "smtpd_recipient_restrictions = permit_mynetworks, permit_sasl_authenticated, permit_auth_destination, reject_unauth_destination" && \
#postconf -e "broken_sasl_auth_clients = no" && \
#echo postfix | saslpasswd2 -c -p postfix



CMD service rsyslog start && service postfix start && tail -f /var/log/mail.log
