FROM golang:1.9.3-alpine3.7 as builder

ADD apk-build /apk-build
RUN chmod +x /apk-build
RUN /apk-build

# deploy bifrost binary
RUN mkdir -p /go/src/github.com/stellar/ \
    && git clone --depth 1 --branch master https://github.com/stellar/go.git /go/src/github.com/stellar/go

ENV BIFROST_VERSION=v0.0.1
ENV BIFROST_URL=https://github.com/stellar/go/releases/download/bifrost-$BIFROST_VERSION/bifrost-linux-amd64
RUN wget $BIFROST_URL
RUN chmod +x bifrost-linux-amd64 && mv bifrost-linux-amd64 /go/bin/bifrost

# deploy db init script
ADD initbifrost /go/src/github.com/stellar/initbifrost
RUN go get github.com/lib/pq \
    && go install github.com/stellar/initbifrost


# -----------------------
FROM alpine:latest

COPY --from=builder /go/bin/bifrost /go/bin/bifrost
COPY --from=builder /go/bin/initbifrost /go/bin/initbifrost
COPY --from=builder /go/src/github.com/stellar/go/services/bifrost/database/migrations/01_init.sql /go/src/github.com/stellar/go/services/bifrost/database/migrations/01_init.sql

ADD build-config /usr/bin/build-config
RUN chmod +x /usr/bin/build-config

RUN ["mkdir", "-p", "/opt/bifrost"]

ADD entry.sh /entry.sh
RUN chmod +x /entry.sh

ADD apk-server /apk-server
RUN chmod +x /apk-server
RUN /apk-server

ADD configs /configs

ENTRYPOINT ["/entry.sh"]

EXPOSE 8800
