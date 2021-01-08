FROM golang:alpine

ARG branch

RUN apk update && apk upgrade && apk add git gcc musl-dev

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

RUN git clone https://github.com/TicketsBot/GoServerCounter .
RUN git checkout $branch
RUN go build -o servercounter cmd/servercounter/servercounter.go

FROM alpine:latest

RUN apk update && apk upgrade

COPY --from=0 /tmp/compile/servercounter /srv/servercounter/servercounter
RUN chmod +x /srv/servercounter/servercounter

RUN adduser container --disabled-password --no-create-home
USER container
WORKDIR /srv/servercounter

CMD ["/srv/servercounter/servercounter"]
