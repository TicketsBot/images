FROM rust:buster

ARG branch

RUN apt-get update && apt-get -y upgrade && apt-get -y install python3 openssl libssl-dev ca-certificates cmake
RUN cargo install sccache

ENV RUSTC_WRAPPER /usr/local/cargo/bin/sccache

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

# Cache bust
ADD https://www.random.org/integers/?num=1&min=-1000000000&max=1000000000&col=1&base=10&format=plain /cachebuster
RUN git clone https://github.com/TicketsBot/tickets.rs .
RUN git checkout $branch
RUN --mount=type=cache,target=/root/.cache/sccache cargo build --release --bin vote_listener

FROM ubuntu:latest

RUN apt-get update && apt-get -y upgrade && apt-get -y install python3 openssl libssl-dev ca-certificates

COPY --from=0 /tmp/compile/target/release/vote_listener /srv/votelistener/votelistener
RUN chmod +x /srv/votelistener/votelistener

RUN useradd -m container
USER container
WORKDIR /srv/votelistener

CMD ["/srv/votelistener/votelistener"]
