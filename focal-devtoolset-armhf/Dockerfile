ARG REPO=mcr.microsoft.com/mirror/docker/library/ubuntu
ARG TAG=20.04
FROM ${REPO}:${TAG}

ARG DEBIAN_FRONTEND=noninteractive
ARG NODE_VERSION="20.18.2"

# ubuntu keeps its i386/amd64 and other architecture repos separate
# the default i386/amd64 packages can be found in http://archive.ubuntu.com while all other architectures are in http://ports.ubuntu.com/
# add the required repos for armhf and only fetch i386/amd64 from archive.ubuntu.com (to prevent apt from erroring about missing architecture support)
RUN sed -i "s/^deb/deb [arch=amd64,i386]/g" /etc/apt/sources.list
RUN echo "deb [arch=arm64,armhf] http://ports.ubuntu.com/ focal main universe multiverse restricted" | tee -a /etc/apt/sources.list
RUN echo "deb [arch=arm64,armhf] http://ports.ubuntu.com/ focal-security main universe multiverse restricted" | tee -a /etc/apt/sources.list
RUN echo "deb [arch=arm64,armhf] http://ports.ubuntu.com/ focal-updates main universe multiverse restricted" | tee -a /etc/apt/sources.list
RUN dpkg --add-architecture armhf
RUN apt-get update && apt-get install -y --no-install-recommends \
  gcc-10-arm-linux-gnueabihf \
  g++-10-arm-linux-gnueabihf \
  binutils-arm-linux-gnueabihf \
  build-essential \
  git \
  pkg-config \
  fakeroot \
  rpm \
  sudo \
  apt-transport-https \
  ca-certificates \
  libx11-dev:armhf \
  libx11-xcb-dev:armhf \
  libxkbfile-dev:armhf \
  libsecret-1-dev:armhf \
  libkrb5-dev:armhf \
  libatomic1:armhf \
  curl \
  gnupg \
  unzip \
  # VSCodium
  jq

# Set python3 as default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1
RUN python --version

# Setup Node.js
RUN curl -L -O https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-armv7l.tar.gz && \
    tar -xzf node-v${NODE_VERSION}-linux-armv7l.tar.gz && \
    cp -R node-v${NODE_VERSION}-linux-armv7l/* /usr/local/ && \
    rm -rf node-v${NODE_VERSION}-linux-armv7l* && \
    node --version

# install yarn & node-gyp
RUN npm install -g yarn node-gyp

# Set gcc-10 as default
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/arm-linux-gnueabihf-gcc-10 10
RUN update-alternatives --install /usr/bin/g++ g++ /usr/bin/arm-linux-gnueabihf-g++-10 10

# Check compiler toolchain
RUN gcc --version
RUN g++ --version

RUN mkdir -p /root/vscode
WORKDIR /root/vscode

ENV AS=/usr/bin/arm-linux-gnueabihf-as \
  AR=/usr/bin/arm-linux-gnueabihf-ar \
  CC=/usr/bin/arm-linux-gnueabihf-gcc-10 \
  CPP=/usr/bin/arm-linux-gnueabihf-cpp-10 \
  CXX=/usr/bin/arm-linux-gnueabihf-g++-10 \
  LD=/usr/bin/arm-linux-gnueabihf-ld \
  PKG_CONFIG_PATH=/usr/lib/arm-linux-gnueabihf/pkgconfig
