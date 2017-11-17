FROM nginx:1.12-alpine
MAINTAINER "Peter Schiffer" <pschiffe@redhat.com>

RUN apk add --no-cache curl

ARG PDNSADMIN_VERSION=master
ARG PDNSADMIN_URL="https://github.com/ngoduykhanh/PowerDNS-Admin/archive/${PDNSADMIN_VERSION}.tar.gz"

RUN mkdir -p /opt/powerdns-admin \
  && curl -sSLk "${PDNSADMIN_URL}" \
    | tar -xzC /opt/powerdns-admin --strip 1 \
  && find /opt/powerdns-admin -path /opt/powerdns-admin/app/static -prune -o -type f -exec rm -f {} + \
  && chown -R root: /opt/powerdns-admin

COPY pdns-nginx.conf /etc/nginx/conf.d/default.conf
