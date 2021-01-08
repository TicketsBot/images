FROM rust:buster

RUN apt-get update && apt-get -y upgrade && apt-get -y install python3 openssl libssl-dev ca-certificates cmake

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

RUN git clone https://github.com/TicketsBot/tickets.rs .
RUN cargo build --release --bin vote_listener

FROM ubuntu:latest

RUN apt-get update && apt-get -y upgrade && apt-get -y install python3 openssl libssl-dev ca-certificates

COPY --from=0 /tmp/compile/target/release/vote_listener /srv/votelistener/votelistener
RUN chmod +x /srv/votelistener/votelistener

RUN useradd -m container
USER container
WORKDIR /srv/votelistener

CMD ["/srv/votelistener/votelistener"]
