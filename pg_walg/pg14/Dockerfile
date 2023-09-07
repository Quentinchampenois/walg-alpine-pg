FROM quentinchampenois/walg:v2.0.1-alpine3.18 AS wal-g
FROM postgres:14-alpine

RUN apk update \
    && apk add gomplate \
    && apk add --no-cache libsodium-dev lzo-dev

COPY --from=wal-g /wal-g /

RUN mkdir -p /etc/postgresql/ \
    && cp /usr/local/share/postgresql/postgresql.conf.sample /etc/postgresql/postgresql.conf.tmpl \
    && sed -ri "s/^#archive_mode = off/archive_mode = {{.Env.ARCHIVE_MODE}}/" /etc/postgresql/postgresql.conf.tmpl \
    && sed -ri "s/^#archive_timeout = 0/archive_timeout = {{.Env.ARCHIVE_TIMEOUT}}/" /etc/postgresql/postgresql.conf.tmpl \
    && sed -ri "s/^#archive_command = ''/archive_command = '\/wal-g wal-push %p'/" /etc/postgresql/postgresql.conf.tmpl \
    && sed -ri "s/^#restore_command = ''/restore_command = '\/wal-g wal-fetch %f %p'/" /etc/postgresql/postgresql.conf.tmpl

ADD docker-entrypoint.sh /docker-entrypoint.sh
RUN chmod u+x /docker-entrypoint.sh

ENTRYPOINT ["/docker-entrypoint.sh"]

ENV PGHOST=/var/run/postgresql \
    PGUSER=postgres \
    POSTGRES_PASSWORD=password \
    ARCHIVE_MODE=off \
    ARCHIVE_TIMEOUT=0 \
    AWS_S3_FORCE_PATH_STYLE=true \
    WALG_COMPRESSION_METHOD=brotli

CMD ["postgres", "-c", "config_file=/etc/postgresql/postgresql.conf"]