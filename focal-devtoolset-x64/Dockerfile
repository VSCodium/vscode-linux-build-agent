ARG REPO=mcr.microsoft.com/mirror/docker/library/ubuntu
ARG TAG=20.04
FROM ${REPO}:${TAG}

ARG DEBIAN_FRONTEND=noninteractive
ARG NODE_VERSION="20.18.2"

RUN apt-get update && apt-get install -y --no-install-recommends \
  gcc-10 \
  g++-10 \
  binutils \
  build-essential \
  git \
  pkg-config \
  fakeroot \
  rpm \
  sudo \
  apt-transport-https \
  ca-certificates \
  libx11-dev \
  libx11-xcb-dev \
  libxkbfile-dev \
  libsecret-1-dev \
  libkrb5-dev \
  libatomic1 \
  curl \
  gnupg \
  unzip \
  # VSCodium
  jq

# Set python3 as default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1
RUN python --version

# Setup Node.js
RUN curl -L -O https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.gz && \
    tar -xzf node-v${NODE_VERSION}-linux-x64.tar.gz && \
    cp -R node-v${NODE_VERSION}-linux-x64/* /usr/local/ && \
    rm -rf node-v${NODE_VERSION}-linux-x64* && \
    node --version

# install yarn & node-gyp
RUN npm install -g yarn node-gyp

# Set gcc-10 as default
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 10
RUN update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-10 10

# Check compiler toolchain
RUN gcc --version
RUN g++ --version

RUN mkdir -p /root/vscode
WORKDIR /root/vscode

RUN ls -la /usr/bin/
RUN ls -la /usr/lib/pkgconfig

ENV AS=/usr/bin/as \
  AR=/usr/bin/ar \
  CC=/usr/bin/gcc-10 \
  CPP=/usr/bin/cpp-10 \
  CXX=/usr/bin/g++-10 \
  LD=/usr/bin/ld \
  PKG_CONFIG_PATH=/usr/lib/pkgconfig
