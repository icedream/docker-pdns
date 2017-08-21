FROM fedora:26
MAINTAINER "Peter Schiffer" <pschiffe@redhat.com>

RUN dnf -y --setopt=tsflags=nodocs install \
    python-pip \
    python2-mysql \
    python-ldap \
    mariadb \
    uwsgi \
    uwsgi-plugin-python \
  && dnf clean all

RUN mkdir -p /opt/powerdns-admin \
  && curl -sSLk https://git.omicroninteractive.com/0x97/powerdns-admin/repository/archive.tar.gz?ref=master \
    | tar -xzC /opt/powerdns-admin --strip 1 \
  && sed -i '/MySQL-python/d' /opt/powerdns-admin/requirements.txt \
  && sed -i '/python-ldap/d' /opt/powerdns-admin/requirements.txt \
  && rm -rf /opt/powerdns-admin/app/static \
  && chown -R root: /opt/powerdns-admin \
  && chown -R uwsgi: /opt/powerdns-admin/upload

WORKDIR /opt/powerdns-admin

RUN pip3 install envtpl \
  && pip install -r requirements.txt \
  && rm -rf ~/.cache/*

ENV PDNS_ADMIN_LOGIN_TITLE="'PDNS'" \
  PDNS_ADMIN_TIMEOUT=10 \
  PDNS_ADMIN_LOG_LEVEL="'INFO'" \
  PDNS_ADMIN_BASIC_ENABLED=True \
  PDNS_ADMIN_SIGNUP_ENABLED=True \
  PDNS_ADMIN_RECORDS_ALLOW_EDIT="['SOA', 'NS', 'A', 'AAAA', 'CNAME', 'MX', 'TXT', 'SRV']"

EXPOSE 9494

VOLUME [ "/opt/powerdns-admin/upload" ]

COPY pdns-admin.ini /etc/uwsgi.d/
RUN chown uwsgi: /etc/uwsgi.d/pdns-admin.ini

COPY config.py.tpl /
COPY docker-cmd.sh /

CMD [ "/docker-cmd.sh" ]