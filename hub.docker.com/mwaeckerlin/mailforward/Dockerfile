FROM mwaeckerlin/smtp-relay-tls
MAINTAINER mwaeckerlin

# setup domains as in the following example:
ENV MAILHOST ""
ENV MAPPINGS ""
ENV LOCAL_DOMAINS ""
ENV GREYLIST ""

EXPOSE 25

ENV CONTAINERNAME "mailforward"
RUN echo mail > /etc/hostname \
 && postconf -e mydestination="localhost" \
 && postconf -e smtpd_use_tls=no \
 && postconf -e smtpd_tls_security_level=none \
 # secure tls https://blog.tinned-software.net/harden-the-ssl-configuration-of-your-mailserver/ \
 && postconf -e smtpd_tls_auth_only=yes \
 && postconf -e 'smtpd_tls_mandatory_protocols = !SSLv2, !SSLv3' \
 && postconf -e 'smtpd_tls_protocols = !SSLv2 !SSLv3' \
 && postconf -e smtpd_tls_mandatory_ciphers=high \
 && postconf -e 'tls_high_cipherlist=EDH+CAMELLIA:EDH+aRSA:EECDH+aRSA+AESGCM:EECDH+aRSA+SHA384:EECDH+aRSA+SHA256:EECDH:+CAMELLIA256:+AES256:+CAMELLIA128:+AES128:+SSLv3:!aNULL:!eNULL:!LOW:!3DES:!MD5:!EXP:!PSK:!DSS:!RC4:!SEED:!ECDSA:CAMELLIA256-SHA:AES256-SHA:CAMELLIA128-SHA:AES128-SHA' \
 && postconf -e smtpd_tls_eecdh_grade=ultra \
 # SPAM Prevention \
 && postconf -e smtpd_hard_error_limit='1' \
 && postconf -e smtpd_helo_required='yes' \
 && postconf -e smtpd_helo_restrictions='permit_tls_clientcerts, permit_sasl_authenticated, reject_invalid_hostname, reject_non_fqdn_hostname, reject_unauth_pipelining' \
 && postconf -e smtpd_sender_restrictions='permit_mynetworks, permit_tls_clientcerts, permit_sasl_authenticated, reject_non_fqdn_sender, reject_unauth_pipelining' \
 && postconf -e smtpd_recipient_restrictions='permit_tls_clientcerts, permit_sasl_authenticated, permit_mynetworks, reject_unknown_recipient_domain, reject_non_fqdn_recipient, reject_unauth_destination, reject_unauth_pipelining, reject_rbl_client ix.dnsbl.manitu.net, reject_rbl_client sbl.spamhaus.org, reject_rbl_client xbl.spamhaus.org' \
 && postconf -e smtpd_client_restrictions='reject_invalid_hostname, reject_rhsbl_sender dbl.spamhaus.org, reject_rhsbl_client dbl.spamhaus.org, reject_rhsbl_helo dbl.spamhaus.org' \
 && postconf -e strict_rfc821_envelopes='yes' \
 && postconf -e smtpd_relay_restrictions='permit_tls_clientcerts, permit_sasl_authenticated, reject_unknown_recipient_domain, reject_non_fqdn_recipient, reject_unauth_destination, reject_unauth_pipelining, reject_rbl_client ix.dnsbl.manitu.net, reject_rbl_client sbl.spamhaus.org, reject_rbl_client xbl.spamhaus.org'
#, check_policy_service inet:127.0.0.1:10023'

VOLUME /etc/letsencrypt

# definitions for our children
ONBUILD RUN mv start.sh start-mailforward.sh
ONBUILD ADD start.sh /start.sh
