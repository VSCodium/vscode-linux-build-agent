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
  gcc-10-aarch64-linux-gnu \
  g++-10-aarch64-linux-gnu \
  binutils-aarch64-linux-gnu \
  build-essential \
  git \
  pkg-config \
  fakeroot \
  rpm \
  sudo \
  apt-transport-https \
  ca-certificates \
  libx11-dev:arm64 \
  libx11-xcb-dev:arm64 \
  libxkbfile-dev:arm64 \
  libsecret-1-dev:arm64 \
  libkrb5-dev:arm64 \
  libatomic1:arm64 \
  libc6:amd64 \
  curl \
  gnupg \
  unzip \
  # VSCodium
  jq

# Set python3 as default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1
RUN python --version

# Setup Node.js
RUN curl -L -O https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-arm64.tar.gz && \
    tar -xzf node-v${NODE_VERSION}-linux-arm64.tar.gz && \
    cp -R node-v${NODE_VERSION}-linux-arm64/* /usr/local/ && \
    rm -rf node-v${NODE_VERSION}-linux-arm64* && \
    node --version

# install yarn & node-gyp
RUN npm install -g yarn node-gyp

# Set gcc-10 as default
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/aarch64-linux-gnu-gcc-10 10
RUN update-alternatives --install /usr/bin/g++ g++ /usr/bin/aarch64-linux-gnu-g++-10 10

# Check compiler toolchain
RUN gcc --version
RUN g++ --version

RUN mkdir -p /root/vscode
WORKDIR /root/vscode

ENV AS=/usr/bin/aarch64-linux-gnu-as \
  AR=/usr/bin/aarch64-linux-gnu-ar \
  CC=/usr/bin/aarch64-linux-gnu-gcc-10 \
  CPP=/usr/bin/aarch64-linux-gnu-cpp-10 \
  CXX=/usr/bin/aarch64-linux-gnu-g++-10 \
  LD=/usr/bin/aarch64-linux-gnu-ld \
  PKG_CONFIG_PATH=/usr/lib/aarch64-linux-gnu/pkgconfig
