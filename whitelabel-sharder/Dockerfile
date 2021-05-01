FROM rust:buster

ARG branch

RUN apt-get update && apt-get -y upgrade && apt-get -y install openssl libssl-dev ca-certificates cmake

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

RUN git clone https://github.com/TicketsBot/tickets.rs .
RUN git checkout $branch
WORKDIR /tmp/compile/sharder
RUN cargo build --release --bin whitelabel --features=whitelabel

FROM ubuntu:latest

RUN apt-get update && apt-get -y upgrade && apt-get -y install python3 openssl libssl-dev ca-certificates

COPY --from=0 /tmp/compile/target/release/whitelabel /srv/sharder/sharder
RUN chmod +x /srv/sharder/sharder

RUN useradd -m container
USER container
WORKDIR /srv/sharder

COPY ./entrypoint.sh /entrypoint.sh

CMD ["/bin/bash", "/entrypoint.sh"]
