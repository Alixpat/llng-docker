FROM yadd/lemonldap-ng-base
LABEL maintainer="Yadd yadd@debian.org>" \
      name="yadd/lemonldap-ng-portal" \
      version="v1.0"

RUN echo "# Install nginx and portal libs" && \
    apt -y update && \
    apt -y dist-upgrade && \
    apt -y --no-install-recommends install nginx \
        lemonldap-ng-fastcgi-server/${DEBIAN_VERSION}-backports \
        liblemonldap-ng-portal-perl/${DEBIAN_VERSION}-backports \
        gsfonts \
        libcrypt-u2f-server-perl libgeoip2-perl \
        libglib-perl libgssapi-perl libhttp-browserdetect-perl \
        libimage-magick-perl liblasso-perl libnet-facebook-oauth2-perl \
        libnet-openid-consumer-perl libnet-openid-server-perl \
        libnet-oauth-perl libsoap-lite-perl fonts-urw-base35 \
        libauthen-webauthn-perl libcrypt-openssl-bignum-perl \
        libconvert-base32-perl libio-string-perl libipc-run-perl \
        libgd-securityimage-perl libmime-tools-perl libnet-ldap-perl \
        libio-socket-timeout-perl libunicode-string-perl liblasso-perl \
        libio-string-perl libemail-sender-perl && \
    apt autoremove -y && apt clean && rm -rf /var/lib/apt/lists/*

RUN echo "# Install nginx configuration files" && \
    cd /etc/nginx/sites-enabled/ && \
    ln -s ../../lemonldap-ng/portal-nginx.conf

RUN (echo ""; echo "daemon off;") >> /etc/nginx/nginx.conf

RUN sed -i -e "s/listen \[::\]:80 default_server\;//" /etc/nginx/sites-enabled/default

COPY install /

EXPOSE 80

CMD ["/usr/sbin/nginx"]
