FROM yadd/lemonldap-ng-base
LABEL maintainer="Yadd yadd@debian.org>" \
      name="yadd/lemonldap-ng-manager" \
      version="v1.0"


RUN echo "# Install nginx and manager libs" && \
    apt -y update && \
    apt -y dist-upgrade && \
    apt -y --no-install-recommends install nginx \
      lemonldap-ng-fastcgi-server/${DEBIAN_VERSION}-backports \
      liblemonldap-ng-manager-perl/${DEBIAN_VERSION}-backports \
      libglib-perl libgssapi-perl libsoap-lite-perl fonts-urw-base35 \
      libcrypt-openssl-bignum-perl \
      libconvert-base32-perl libio-string-perl libipc-run-perl \
      libgd-securityimage-perl libmime-tools-perl libnet-ldap-perl \
      libio-socket-timeout-perl libunicode-string-perl \
      libio-string-perl && \
    mv /etc/dpkg/dpkg.cfg.d /tmp/ && \
    apt -y --no-install-recommends install \
      lemonldap-ng-doc/${DEBIAN_VERSION}-backports && \
    mv /tmp/dpkg.cfg.d /etc/dpkg/ && \
    apt autoremove -y && apt clean && rm -rf /var/lib/apt/lists/*

RUN echo "# Install nginx configuration files" && \
    cd /etc/nginx/sites-enabled/ && \
    ln -s ../../lemonldap-ng/manager-nginx.conf

RUN (echo ""; echo "daemon off;") >> /etc/nginx/nginx.conf

RUN sed -i -e "s/listen \[::\]:80 default_server\;//" /etc/nginx/sites-enabled/default

COPY install /

EXPOSE 80

CMD ["/usr/sbin/nginx"]
