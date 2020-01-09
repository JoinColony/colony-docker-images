FROM node:10.16

# @NOTE Declare the commit hash build variable, so that it gets picked up by the conditional
ARG COMMIT_HASH
ARG JWT_SECRET

RUN if [ -z "$JWT_SECRET" ]; then echo "Secret build argument NOT SET. Stopping..."; exit 1; else : ; fi

# Build arguments that have default values
ARG ETH_NETWORK=http://localhost:8545
ARG NETWORK_CONTRACT_ADDRESS=0xde3EDaBA7c15F845F59D1058eCD70Ed33FfdB2b9
ARG APOLLO_PORT=3000

# Update the apt cache
RUN apt-get clean
RUN apt-get update

# Apt-utils needs to be in before installing the rest
RUN apt-get install -y \
  locales \
  apt-utils \
  build-essential \
  curl \
  file \
  zip

# Reconfigure locales
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
RUN locale-gen

# Install version 1.0.0 of libssl
# Required by Mongo
RUN wget http://security.debian.org/debian-security/pool/updates/main/o/openssl/libssl1.0.0_1.0.1t-1+deb8u12_amd64.deb
RUN dpkg -i libssl1.0.0_1.0.1t-1+deb8u12_amd64.deb

# Install MongoDB
RUN wget https://repo.mongodb.org/apt/ubuntu/dists/xenial/mongodb-org/4.2/multiverse/binary-amd64/mongodb-org-server_4.2.2_amd64.deb
RUN dpkg -i mongodb-org-server_4.2.2_amd64.deb

# Clone the repo
RUN git clone "https://github.com/JoinColony/colonyServer.git"
WORKDIR /colonyServer
RUN if [ ! -z "$COMMIT_HASH" ]; then git checkout $COMMIT_HASH; fi

# Setup the environment variables
RUN echo "DB_NAME=colonyCentralizedMetadata\n" \
  "DB_URL=mongodb://0.0.0.0:27017\n" \
  "DISABLE_EXPIRY_CHECK=false\n" \
  "JWT_SECRET=$JWT_SECRET\n" \
  "ETH_NETWORK=$ETH_NETWORK\n" \
  "NETWORK_CONTRACT_ADDRESS=$NETWORK_CONTRACT_ADDRESS\n" \
  "APOLLO_PORT=$APOLLO_PORT\n" > .env

# Setup the server modules
RUN npm i

# Create mongo's data and logs folders
RUN mkdir mongo-data logs

# Start the mongo server
RUN npm run db:start:prod && npm run db:setup && npm run prod &
# Debug
# RUN cat logs/mongod.log

# Setup the mongo database collections
# RUN npm run db:setup

# Expose the Apollo Server's port
EXPOSE 3000

# Start the Express Server
# RUN npm run prod

# READY TO GO !
