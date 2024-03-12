FROM alpine:3.18 as base
ARG PGBOUNCER_VERSION=1.22.1
# hadolint ignore=DL3018
RUN apk add -U --no-cache --upgrade busybox ca-certificates postgresql-client libevent c-ares


FROM base as builder
# hadolint ignore=SC2086,DL3003,DL3018
RUN \
  apk add -U --no-cache autoconf automake curl gcc libc-dev libevent-dev libtool make openssl-dev pkgconfig c-ares-dev && \
  curl -o  /tmp/pgbouncer-$PGBOUNCER_VERSION.tar.gz -L "https://github.com/pgbouncer/pgbouncer/releases/download/pgbouncer_${PGBOUNCER_VERSION//./_}/pgbouncer-$PGBOUNCER_VERSION.tar.gz" && \
  tar xvfz /tmp/pgbouncer-$PGBOUNCER_VERSION.tar.gz -C /tmp && \
  cd /tmp/pgbouncer-$PGBOUNCER_VERSION && \
  ./configure --prefix=/usr && \
  make


FROM base as prod

RUN \
  mkdir -p /etc/pgbouncer /var/log/pgbouncer /var/run/pgbouncer && \
  touch /etc/pgbouncer/userlist.txt && \
  chown -R postgres /var/run/pgbouncer /etc/pgbouncer

COPY --from=builder /tmp/pgbouncer-$PGBOUNCER_VERSION/pgbouncer /usr/bin
COPY --from=builder /tmp/pgbouncer-$PGBOUNCER_VERSION/etc/pgbouncer.ini /etc/pgbouncer/pgbouncer.ini.example
COPY --from=builder /tmp/pgbouncer-$PGBOUNCER_VERSION/etc/userlist.txt /etc/pgbouncer/userlist.txt.example
COPY entrypoint.sh /entrypoint.sh

USER postgres
EXPOSE 5432
# ENTRYPOINT ["/entrypoint.sh"]
CMD ["/usr/bin/pgbouncer", "/etc/pgbouncer/pgbouncer.ini"]
