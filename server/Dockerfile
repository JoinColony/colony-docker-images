FROM node:10.16

# @NOTE Declare the commit hash build variable, so that it gets picked up by the conditional
ARG COMMIT_HASH
ARG JWT_SECRET
ARG INFURA_ID

RUN if [ -z "$JWT_SECRET" ]; then echo "JWT_SECRET build argument was NOT SET. Stopping..."; exit 1; else : ; fi
RUN if [ -z "$INFURA_ID" ]; then echo "INFURA_ID build argument was NOT SET. Stopping..."; exit 1; else : ; fi

# Build arguments that have default values
ARG NETWORK_CONTRACT_ADDRESS=0xde3EDaBA7c15F845F59D1058eCD70Ed33FfdB2b9
ARG APOLLO_PORT=3000
ARG NETWORK=goerli
ARG DISABLE_EXPIRY_CHECK=false
ARG DISABLE_AUTH_CHECK=false

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
  "DB_URL=mongodb://127.0.0.1:27017\n" \
  "DISABLE_EXPIRY_CHECK=$DISABLE_EXPIRY_CHECK\n" \
  "DISABLE_AUTH_CHECK=$DISABLE_AUTH_CHECK\n" \
  "JWT_SECRET=$JWT_SECRET\n" \
  "NETWORK=$NETWORK\n" \
  "NETWORK_CONTRACT_ADDRESS=$NETWORK_CONTRACT_ADDRESS\n" \
  "INFURA_ID=$INFURA_ID\n" \
  "APOLLO_PORT=$APOLLO_PORT\n" > .env

# Setup the server modules
RUN npm i

# Build the server files
RUN npm run build

# Create mongo's data and logs folders
RUN mkdir mongo-data logs

# Expose the Apollo Server's port
EXPOSE 3000

# Start the Express Server, The MongoDB Server and setup the Database schema
CMD ["npm", "run", "prod"]

# READY TO GO !
