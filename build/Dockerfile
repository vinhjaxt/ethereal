# Build Geth in a stock Go builder container
FROM golang:alpine as builder

# TARGET={bootnode|fullnode|miningnode}
ENV TARGET=fullnode

RUN apk add --no-cache make gcc musl-dev linux-headers git

RUN mkdir -p /opt/go-ethereum

RUN git clone https://github.com/ethereum/go-ethereum.git /opt/go-ethereum

WORKDIR /opt/go-ethereum

RUN git checkout master

RUN make all


# Pull Geth into a second stage deploy alpine container
FROM alpine:latest

RUN apk add --no-cache ca-certificates
COPY --from=builder /opt/go-ethereum/build/bin /opt/geth

EXPOSE 8545 8546 8547 30303 30303/udp

# RUN
RUN apk --no-cache --update add bash bash-completion

ENV NODE_NAME=""
ENV GENESIS_FILE="genesis.json"
ENV MINERTHREADS=1

# TARGET={bootnode|fullnode|miningnode}
ENV TARGET=fullnode

RUN mkdir /opt/shared
VOLUME /opt/shared

RUN mkdir /opt/genesis
VOLUME /opt/genesis


COPY ./run.sh /opt/
RUN chmod +x /opt/run.sh
WORKDIR /opt

RUN ls -alvh /usr/local/bin

ENTRYPOINT ["bash", "/opt/run.sh"]
CMD []