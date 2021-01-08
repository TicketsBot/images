FROM rust:latest

RUN apt-get update && apt-get -y upgrade && apt-get -y install git openssl libssl-dev ca-certificates
WORKDIR /tmp/compile/
RUN git clone https://github.com/twilight-rs/http-proxy .
RUN cargo build --release

FROM ubuntu:latest

RUN apt-get update && apt-get -y upgrade && apt-get -y install git openssl libssl-dev ca-certificates

COPY --from=0 /tmp/compile/target/release/twilight-http-proxy /srv/proxy/proxy
RUN chmod +x /srv/proxy/proxy

RUN useradd -m container
USER container
WORKDIR /srv/proxy

CMD ["/srv/proxy/proxy"]
