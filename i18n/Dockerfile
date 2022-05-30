FROM golang:alpine

ARG branch

RUN apk update && apk upgrade && apk add git gcc musl-dev

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

RUN git clone https://github.com/TicketsBot/translationgenerator .
RUN git checkout $branch
RUN go build -o i18n cmd/translationgenerator/main.go

FROM alpine:latest

RUN apk update && apk upgrade

COPY --from=0 /tmp/compile/i18n /srv/i18n/i18n
RUN chmod +x /srv/i18n/i18n

COPY --from=0 /tmp/compile/public /srv/i18n/public
COPY ./en.json /srv/i18n/en.json

RUN adduser container --disabled-password --no-create-home
USER container
WORKDIR /srv/i18n

CMD ["/srv/i18n/i18n"]
