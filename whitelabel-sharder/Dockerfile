# syntax = docker/dockerfile:experimental
FROM rust:buster

ARG branch

RUN apt-get update && apt-get -y upgrade && apt-get -y install openssl libssl-dev ca-certificates cmake
RUN cargo install sccache

ENV RUSTC_WRAPPER /usr/local/cargo/bin/sccache

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

# Cache bust
ADD https://www.random.org/integers/?num=1&min=-1000000000&max=1000000000&col=1&base=10&format=plain /cachebuster
RUN git clone https://github.com/TicketsBot/tickets.rs .
RUN git checkout $branch
WORKDIR /tmp/compile/sharder
RUN --mount=type=cache,target=/root/.cache/sccache cargo build --release --bin whitelabel --features=whitelabel

FROM ubuntu:latest

RUN apt-get update && apt-get -y upgrade && apt-get -y install python3 openssl libssl-dev ca-certificates

COPY --from=0 /tmp/compile/target/release/whitelabel /srv/sharder/sharder
RUN chmod +x /srv/sharder/sharder

RUN useradd -m container
USER container
WORKDIR /srv/sharder

COPY ./entrypoint.sh /entrypoint.sh

CMD ["/bin/bash", "/entrypoint.sh"]
