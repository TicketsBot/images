FROM golang:alpine

ARG branch

RUN apk update && apk upgrade && apk add git zlib-dev gcc musl-dev

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

RUN git clone https://github.com/TicketsBot/GoPanel .
RUN git checkout $branch
RUN go build -o panel cmd/panel/main.go

FROM alpine:latest

RUN apk update && apk upgrade

COPY --from=0 /tmp/compile/panel /srv/panel/panel
RUN chmod +x /srv/panel/panel

COPY --from=0 /tmp/compile/public /srv/panel/public
COPY --from=0 /tmp/compile/emojis.json /srv/panel/emojis.json

RUN adduser container --disabled-password --no-create-home
USER container
WORKDIR /srv/panel

CMD ["/srv/panel/panel"]
