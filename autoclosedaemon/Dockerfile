FROM golang:alpine

ARG branch

RUN apk update && apk upgrade && apk add git zlib-dev gcc musl-dev

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

RUN git clone https://github.com/TicketsBot/autoclosedaemon .
RUN git checkout $branch
RUN go build -o daemon cmd/autoclosedaemon/main.go

FROM alpine:latest

RUN apk update && apk upgrade

COPY --from=0 /tmp/compile/daemon /srv/daemon/daemon
RUN chmod +x /srv/daemon/daemon

RUN adduser container --disabled-password --no-create-home
USER container
WORKDIR /srv/daemon

CMD ["/srv/daemon/daemon"]
