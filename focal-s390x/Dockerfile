ARG REPO=mcr.microsoft.com/mirror/docker/library/ubuntu
ARG TAG=20.04
FROM ${REPO}:${TAG}

ARG DEBIAN_FRONTEND=noninteractive

# ubuntu keeps its i386/amd64 and other architecture repos separate
# the default i386/amd64 packages can be found in http://archive.ubuntu.com while all other architectures are in http://ports.ubuntu.com/
# add the required repos for s390x and only fetch i386/amd64 from archive.ubuntu.com (to prevent apt from erroring about missing architecture support)
RUN sed -i "s/^deb/deb [arch=amd64,i386]/g" /etc/apt/sources.list
RUN echo "deb [arch=s390x] http://ports.ubuntu.com/ focal main universe multiverse restricted" | tee -a /etc/apt/sources.list
RUN echo "deb [arch=s390x] http://ports.ubuntu.com/ focal-security main universe multiverse restricted" | tee -a /etc/apt/sources.list
RUN echo "deb [arch=s390x] http://ports.ubuntu.com/ focal-updates main universe multiverse restricted" | tee -a /etc/apt/sources.list
RUN dpkg --add-architecture s390x
RUN apt-get update && apt-get install -y --no-install-recommends \
  gcc-10-s390x-linux-gnu \
  g++-10-s390x-linux-gnu \
  binutils-s390x-linux-gnu \
  build-essential \
  git \
  pkg-config \
  fakeroot \
  rpm \
  sudo \
  apt-transport-https \
  ca-certificates \
  libx11-dev:s390x \
  libx11-xcb-dev:s390x \
  libxkbfile-dev:s390x \
  libsecret-1-dev:s390x \
  libkrb5-dev:s390x \
  libatomic1:s390x \
  curl \
  gnupg \
  unzip \
  # VSCodium
  jq

# Set python3 as default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1
RUN python --version

# Yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install -y yarn

# Node.js
RUN curl --silent --location https://deb.nodesource.com/setup_22.x | sudo -E bash -
RUN apt-get update && apt-get install -y nodejs
RUN npm install -g npm@latest
RUN npm install -g node-gyp

# Set gcc-10 as default
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/s390x-linux-gnu-gcc-10 10
RUN update-alternatives --install /usr/bin/g++ g++ /usr/bin/s390x-linux-gnu-g++-10 10

# Check compiler toolchain
RUN gcc --version
RUN g++ --version

ENV AS=/usr/bin/s390x-linux-gnu-as \
  AR=/usr/bin/s390x-linux-gnu-ar \
  CC=/usr/bin/s390x-linux-gnu-gcc-10 \
  CPP=/usr/bin/s390x-linux-gnu-cpp-10 \
  CXX=/usr/bin/s390x-linux-gnu-g++-10 \
  LD=/usr/bin/s390x-linux-gnu-ld \
  PKG_CONFIG_PATH=/usr/lib/s390x-linux-gnu/pkgconfig
