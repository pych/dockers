FROM mikefarah/yq:4.27.5 AS yq

FROM golang:1.17.13-buster AS tools

RUN mkdir -p /tools/{git,bin,tmp}
# build restic
RUN apt update -y && apt install -y git \
    && git clone -b v0.14.0 https://github.com/restic/restic.git /tools/git/restic \
    && cd /tools/git/restic && go run build.go

FROM debian:buster-slim

COPY --from=yq /usr/bin/yq  /usr/bin/yq
COPY --from=tools /tools/git/restic/restic /usr/bin/restic
