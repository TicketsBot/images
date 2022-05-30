# syntax = docker/dockerfile:experimental
# Build container
FROM rust:buster

ARG branch

RUN apt-get update && apt-get -y upgrade && apt-get -y install git openssl libssl-dev ca-certificates
RUN cargo install sccache

ENV RUSTC_WRAPPER /usr/local/cargo/bin/sccache

WORKDIR /tmp/compile/

# Cache bust
ADD https://www.random.org/integers/?num=1&min=-1000000000&max=1000000000&col=1&base=10&format=plain /cachebuster
RUN git clone https://github.com/rxdn/http-proxy .
RUN git checkout $branch
RUN --mount=type=cache,target=/root/.cache/sccache cargo build --release --features=expose-metrics

# Prod container
FROM ubuntu:latest

RUN apt-get update && apt-get -y upgrade && apt-get -y install git openssl libssl-dev ca-certificates

COPY --from=0 /tmp/compile/target/release/twilight-http-proxy /srv/proxy/proxy
RUN chmod +x /srv/proxy/proxy

RUN useradd -m container
USER container
WORKDIR /srv/proxy

CMD ["/srv/proxy/proxy"]
