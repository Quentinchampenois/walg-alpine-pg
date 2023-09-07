FROM golang:1.20-alpine3.18 as builder

ENV WALG_VERSION=v2.0.1 \
    BROTLI_VERSION=v1.0.9 \
    USE_BROTLI=1 \
    USE_LIBSODIUM=1 \
    USE_LZO=1

RUN apk add --no-cache make git bash cmake wget curl build-base libsodium-dev lzo lzo-dev

RUN git clone https://github.com/wal-g/wal-g.git

WORKDIR wal-g

RUN git checkout $WALG_VERSION

RUN make deps \
    && make pg_build \
    && make pg_install

FROM alpine:3.18

RUN apk add --no-cache libsodium-dev lzo-dev
COPY --from=builder /wal-g /