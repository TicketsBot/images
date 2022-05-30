# syntax = docker/dockerfile:experimental
# Build
FROM golang:buster

ARG branch

RUN apt-get update && apt-get upgrade -y && apt-get install -y ca-certificates git zlib1g-dev

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

# Cache bust
ADD https://www.random.org/integers/?num=1&min=-1000000000&max=1000000000&col=1&base=10&format=plain /cachebuster
RUN git clone --recurse-submodules https://github.com/TicketsBot/worker.git .
RUN cd locale && git pull origin master 
RUN git checkout $branch
RUN --mount=type=cache,target=/root/.cache/go-build go build -tags=jsoniter cmd/worker/main.go

# Prod container
FROM ubuntu:latest

RUN apt-get update && apt-get upgrade -y && apt-get install -y ca-certificates

COPY --from=0 /tmp/compile/locale /srv/worker/locale
COPY --from=0 /tmp/compile/main /srv/worker/worker
RUN chmod +x /srv/worker/worker

RUN useradd -m container
USER container
WORKDIR /srv/worker

CMD ["/srv/worker/worker"]
