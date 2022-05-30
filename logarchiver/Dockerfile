FROM golang:alpine

ARG branch

RUN apk update && apk upgrade && apk add git zlib-dev gcc musl-dev

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

RUN git clone https://github.com/TicketsBot/logarchiver .
RUN git checkout $branch
RUN go build -o logarchiver cmd/logarchiver/main.go

FROM alpine:latest

RUN apk update && apk upgrade

COPY --from=0 /tmp/compile/logarchiver /srv/logarchiver/logarchiver
RUN chmod +x /srv/logarchiver/logarchiver

COPY --from=0 /tmp/compile/public /srv/logarchiver/public

RUN adduser container --disabled-password --no-create-home
USER container
WORKDIR /srv/logarchiver

CMD ["/srv/logarchiver/logarchiver"]
