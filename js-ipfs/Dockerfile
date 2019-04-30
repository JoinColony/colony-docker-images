FROM node:10-stretch-slim

MAINTAINER Christian Maniewski <chris@colony.io>

ENV IPFS_VERSION=0.35.0

ENV IPFS_WRTC_LINUX_WINDOWS=1
# That leads to an error so we disable that 
# ENV IPFS_MONITORING=1
ENV IPFS_PATH=/root/.jsipfs
ENV IPFS_API_HOST=0.0.0.0

EXPOSE 4002
EXPOSE 4003
EXPOSE 5002
EXPOSE 9090

ENV BUILD_DEPS='build-essential git python libnspr4 libnspr4-dev libnss3'
ENV npm_config_user=root

RUN apt-get update \
  && apt-get install --yes $BUILD_DEPS \
  && rm -rf /var/lib/apt/lists/* \
  && npm install -g ipfs@$IPFS_VERSION \
  && npm install -g wrtc@0.3.7 \ 
  && apt-get purge --yes $BUILD_DEPS \
  && rm -rf /usr/share/doc /usr/share/locale \
  && rm -rf /usr/local/share/.cache

COPY ./run /root/run

ENTRYPOINT ["/root/run"]

CMD ["daemon"]
