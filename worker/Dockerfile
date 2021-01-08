# Build
FROM golang:buster

ARG branch

RUN apt-get update && apt-get upgrade -y && apt-get install -y ca-certificates git zlib1g-dev

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile
RUN git clone https://github.com/TicketsBot/worker.git .
RUN git checkout $branch
RUN go build cmd/worker/main.go

# Prod container
FROM ubuntu:latest

RUN apt-get update && apt-get upgrade -y && apt-get install -y ca-certificates

COPY --from=0 /tmp/compile/main /srv/worker/worker
RUN chmod +x /srv/worker/worker

RUN useradd -m container
USER container
WORKDIR /srv/worker

CMD ["/srv/worker/worker"]
