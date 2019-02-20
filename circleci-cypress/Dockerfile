FROM circleci/node:10.14

USER root

# Update the apt cache
RUN apt-get update

# install cypress dependencies
RUN apt-get install -y \
  libgtk2.0-0 \
  libnotify-dev \
  libgconf-2-4 \
  libnss3 \
  libnss3-tools \
  libxss1 \
  libasound2 \
  xvfb \
  vnc4server \
  metacity \
  build-essential \
  curl \
  file

RUN node --version
RUN echo "force new chrome here"

# install Chromebrowser
RUN \
  wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
  echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list && \
  apt-get install -y dbus-x11 google-chrome-stable && \
  rm -rf /var/lib/apt/lists/*

# We commented that out and hope it will work anyways 🤞
# "fake" dbus address to prevent errors
# https://github.com/SeleniumHQ/docker-selenium/issues/87
# ENV DBUS_SESSION_BUS_ADDRESS=/dev/null

# Add zip utility - it comes in very handy
RUN apt-get install -y zip

# versions of local tools
RUN node -v
RUN npm -v
RUN yarn -v
RUN google-chrome --version
RUN zip --version
RUN git --version

USER circleci

# Install Linuxbrew
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"
# Add Linuxbrew to the current .profile
RUN \
  test -d ~/.linuxbrew && eval $(~/.linuxbrew/bin/brew shellenv) && \
  test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv) && \
  test -r ~/.bash_profile && echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.bash_profile && \
  echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.profile

# Install mkcert using Linuxbrew
RUN brew install mkcert
# Generate local CA certificates
RUN mkcert -install
