FROM golang:1.9.3-alpine3.7 as builder

ADD apk-build /apk-build
RUN chmod +x /apk-build
RUN /apk-build

# deploy bifrost binary
RUN mkdir -p /go/src/github.com/stellar/ \
    && git clone --depth 1 --branch bifrost-v0.0.2 https://github.com/stellar/go.git /go/src/github.com/stellar/go \
    && cd /go/src/github.com/stellar/go \
    && curl https://glide.sh/get | sh \
    && glide install \
    && go install github.com/stellar/go/services/bifrost

# -----------------------
FROM alpine:latest

COPY --from=builder /go/bin/bifrost /go/bin/bifrost

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
