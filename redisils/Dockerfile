FROM mikefarah/yq:4.27.5 AS yq

FROM golang:1.17.13-buster AS tools

RUN mkdir -p /tools/{git,bin,tmp}
# build restic
RUN apt update -y && apt install -y git \
    && git clone -b v0.14.0 https://github.com/restic/restic.git /tools/git/restic \
    && cd /tools/git/restic && go run build.go

RUN git clone -b v2.2.11 https://github.com/shvc/s3cli.git /tools/git/s3cli \
    && cd /tools/git/s3cli && go build

RUN git clone -b 0.8.18 https://github.com/barnybug/cli53.git /tools/git/cli53 \
    && cd /tools/git/cli53 && go build ./cmd/cli53

RUN git clone -b v0.2.1 https://github.com/aptible/supercronic.git /tools/git/supercronic \
    && cd /tools/git/supercronic && go build -i

FROM debian:buster-slim

COPY --from=yq /usr/bin/yq  /usr/bin/yq
COPY --from=tools /tools/git/restic/restic /usr/bin/restic
COPY --from=tools /tools/git/s3cli/s3cli /usr/bin/s3cli
COPY --from=tools /tools/git/cli53/cli53 /usr/bin/cli53
COPY --from=tools /tools/git/supercronic/supercronic /usr/bin/supercronic

