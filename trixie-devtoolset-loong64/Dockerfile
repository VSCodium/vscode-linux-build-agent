ARG REPO=loongarch64/debian
ARG TAG=latest
FROM ${REPO}:${TAG}

ARG DEBIAN_FRONTEND=noninteractive
ARG NODE_VERSION="20.16.0"

RUN apt-get update && \
  apt-get install -y --no-install-recommends \
    binutils \
    build-essential \
    # build-essential will offer gcc-14 and g++-14, which is overly new for us.
    gcc-12 \
    g++-12 \
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

# set gcc-12 and g++-12 as default (gcc doesn't support loong64 until 12.1)
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-12 1 && \
  update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-12 1

# set python3 as default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1 && \
  python --version

# setup Node.js
RUN curl -L -O https://unofficial-builds.nodejs.org/download/release/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-loong64.tar.gz && \
    tar -xzf node-v${NODE_VERSION}-linux-loong64.tar.gz && \
    cp -R node-v${NODE_VERSION}-linux-loong64/* /usr/local/ && \
    rm -rf node-v${NODE_VERSION}-linux-loong64* && \
    node --version

# install yarn & node-gyp
RUN npm install -g yarn node-gyp

RUN mkdir -p /root/vscode
WORKDIR /root/vscode
