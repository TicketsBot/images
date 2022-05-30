# syntax = docker/dockerfile:experimental
# Build
FROM golang:alpine

ARG branch

RUN apk update && apk upgrade && apk add git zlib-dev gcc musl-dev

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

ADD https://www.random.org/integers/?num=1&min=-1000000000&max=1000000000&col=1&base=10&format=plain /cachebuster
RUN git clone https://github.com/TicketsBot/autoclosedaemon .
RUN git checkout $branch
RUN --mount=type=cache,target=/root/.cache/go-build go build -o main cmd/autoclosedaemon/main.go

# Prod
FROM alpine:latest

RUN apk update && apk upgrade

COPY --from=0 /tmp/compile/main /srv/daemon/daemon
RUN chmod +x /srv/daemon/daemon

RUN adduser container --disabled-password --no-create-home
USER container
WORKDIR /srv/daemon

CMD ["/srv/daemon/daemon"]
