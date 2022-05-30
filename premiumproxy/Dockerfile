FROM rust:buster

ARG branch

RUN apt-get update && apt-get -y upgrade && apt-get -y install git openssl libssl-dev ca-certificates cmake

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

RUN git clone https://github.com/TicketsBot/tickets.rs .
RUN git checkout $branch
RUN cargo build --release --bin patreon-proxy

FROM ubuntu:latest

RUN apt-get update && apt-get -y upgrade && apt-get -y install openssl libssl-dev ca-certificates

COPY --from=0 /tmp/compile/target/release/patreon-proxy /srv/proxy/proxy
RUN chmod +x /srv/proxy/proxy

RUN useradd -m container
USER container
WORKDIR /srv/proxy

CMD ["/srv/proxy/proxy"]
