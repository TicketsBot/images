FROM golang:alpine

RUN apk update && apk upgrade && apk add git

RUN mkdir /tmp/compile
WORKDIR /tmp/compile

RUN git clone https://github.com/TicketsBot/Website.git .
RUN go build cmd/website/main.go

FROM alpine:latest

RUN apk update && apk upgrade

COPY --from=0 /tmp/compile/main /srv/website/website
RUN chmod +x /srv/website/website

COPY --from=0 /tmp/compile/public /srv/website/public
COPY --from=0 /tmp/compile/config.toml /srv/website/config.toml

RUN adduser container --disabled-password --no-create-home
USER container
WORKDIR /srv/website

CMD ["/srv/website/website"]
