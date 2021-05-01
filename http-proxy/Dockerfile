FROM rust:latest

ARG branch

RUN apt-get update && apt-get -y upgrade && apt-get -y install git openssl libssl-dev ca-certificates
WORKDIR /tmp/compile/
RUN git clone https://github.com/rxdn/http-proxy .
RUN git checkout $branch
RUN cargo build --release --features=expose-metrics

FROM ubuntu:latest

RUN apt-get update && apt-get -y upgrade && apt-get -y install git openssl libssl-dev ca-certificates

COPY --from=0 /tmp/compile/target/release/twilight-http-proxy /srv/proxy/proxy
RUN chmod +x /srv/proxy/proxy

RUN useradd -m container
USER container
WORKDIR /srv/proxy

CMD ["/srv/proxy/proxy"]
