ARG REPO=debian
ARG TAG=unstable
FROM ${REPO}:${TAG}

ARG DOCKER_CE_VERSION=27.3.1

# Disable user so it can run on GH (VSCodium)
# RUN groupadd --gid 1000 builduser \
#   && useradd --uid 1000 --gid builduser --shell /bin/bash --create-home builduser

ENV DEBIAN_FRONTEND=noninteractive
RUN dpkg --add-architecture loong64 && \
  apt-get update && \
  apt-get install -y debian-ports-archive-keyring && \
  echo "deb [arch=loong64] http://ftp.ports.debian.org/debian-ports sid main" >> /etc/apt/sources.list && \
  apt-get update && \
  apt-get install -y --no-install-recommends \
    gcc-14-loongarch64-linux-gnu \
    g++-14-loongarch64-linux-gnu \
    binutils-loongarch64-linux-gnu \
    build-essential \
    git \
    pkg-config \
    fakeroot \
    rpm \
    sudo \
    apt-transport-https \
    ca-certificates \
    libx11-dev:loong64 \
    libx11-xcb-dev:loong64 \
    libxkbfile-dev:loong64 \
    libsecret-1-dev:loong64 \
    libkrb5-dev:loong64 \
    libatomic1:loong64 \
    curl \
    gnupg \
    unzip \
    # VSCodium
    jq

# set python3 as default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1 && \
  python --version

# setup Node.js
RUN curl --silent --location https://deb.nodesource.com/setup_20.x | sudo -E bash - && \
  apt-get update && \
  apt-get install -y nodejs && \
  npm install -g npm@latest node-gyp yarn

# Install docker client
RUN curl -LO https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKER_CE_VERSION}.tgz && \
  tar -xzvf docker-${DOCKER_CE_VERSION}.tgz && \
  mv docker/* /usr/bin/

# Check compiler toolchain
RUN gcc --version && \
  g++ --version

# Disable user so it can run on GH (VSCodium)
# USER builduser
# WORKDIR /home/builduser

ENV AS=/usr/bin/loongarch64-linux-gnu-as \
  AR=/usr/bin/loongarch64-linux-gnu-ar \
  CC=/usr/bin/loongarch64-linux-gnu-gcc-14 \
  CPP=/usr/bin/loongarch64-linux-gnu-cpp-14 \
  CXX=/usr/bin/loongarch64-linux-gnu-g++-14 \
  LD=/usr/bin/loongarch64-linux-gnu-ld \
  PKG_CONFIG_PATH=/usr/lib/loongarch64-linux-gnu/pkgconfig
