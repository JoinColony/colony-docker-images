FROM node:10.16-slim

MAINTAINER Christian Maniewski <chris@colony.io>

RUN mkdir /rendezvous

COPY package.json /rendezvous
COPY package-lock.json /rendezvous

RUN apt-get update && apt-get install -y build-essential git python && \
    cd /rendezvous && \
    npm install

EXPOSE 9090

ENTRYPOINT /rendezvous/node_modules/.bin/star-signal
