MAINTAINER Christian Maniewski <chris@colony.io>

FROM node:10.16-slim AS builder

RUN mkdir /rendezvous

COPY package.json /rendezvous
COPY package-lock.json /rendezvous

RUN apt-get update && apt-get install -y build-essential git python && \
    cd /rendezvous && \
    npm install

FROM node:10.16-slim

COPY --from=builder /rendezvous /rendezvous

EXPOSE 9090

ENTRYPOINT /rendezvous/node_modules/.bin/rendezvous
