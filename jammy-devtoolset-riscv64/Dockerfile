ARG REPO=mcr.microsoft.com/mirror/docker/library/ubuntu
ARG TAG=22.04
FROM ${REPO}:${TAG}

ARG DEBIAN_FRONTEND=noninteractive
ARG NODE_VERSION="24.18.0"
ARG NODE_LOCAL_REVISION="1"
ARG NODE_SHA256="65109e57e34279184efcca5888bc156cff3d24bf8eead73de16a28a313c18884"

# ubuntu keeps its i386/amd64 and other architecture repos separate
# the default i386/amd64 packages can be found in http://archive.ubuntu.com while all other architectures are in http://ports.ubuntu.com/
# add the required repos for riscv64 and only fetch i386/amd64 from archive.ubuntu.com (to prevent apt from erroring about missing architecture support)
RUN sed -i "s/^deb/deb [arch=amd64,i386]/g" /etc/apt/sources.list
RUN echo "deb [arch=riscv64] http://ports.ubuntu.com/ jammy main universe multiverse restricted" | tee -a /etc/apt/sources.list
RUN echo "deb [arch=riscv64] http://ports.ubuntu.com/ jammy-security main universe multiverse restricted" | tee -a /etc/apt/sources.list
RUN echo "deb [arch=riscv64] http://ports.ubuntu.com/ jammy-updates main universe multiverse restricted" | tee -a /etc/apt/sources.list
RUN dpkg --add-architecture riscv64
RUN apt-get update && apt-get install -y --no-install-recommends \
  gcc-10-riscv64-linux-gnu \
  g++-10-riscv64-linux-gnu \
  binutils-riscv64-linux-gnu \
  build-essential \
  git \
  pkg-config \
  fakeroot \
  rpm \
  sudo \
  apt-transport-https \
  ca-certificates \
  libx11-dev:riscv64 \
  libx11-xcb-dev:riscv64 \
  libxkbfile-dev:riscv64 \
  libsecret-1-dev:riscv64 \
  libkrb5-dev:riscv64 \
  libatomic1:riscv64 \
  curl \
  gnupg \
  unzip \
  # VSCodium
  jq

# set python3 as default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1
RUN python --version

# Setup Node.js
RUN set -eux; \
  NODE_DISTRO="node-v${NODE_VERSION}-linux-riscv64-local${NODE_LOCAL_REVISION}"; \
  curl -fsSLO "https://github.com/riscv-forks/node-riscv/releases/download/v${NODE_VERSION}-riscv64.${NODE_LOCAL_REVISION}/${NODE_DISTRO}.tar.gz"; \
  echo "${NODE_SHA256}  ${NODE_DISTRO}.tar.gz" | sha256sum -c -; \
  tar -xzf "${NODE_DISTRO}.tar.gz"; \
  cp -R "${NODE_DISTRO}"/* /usr/local/; \
  rm -rf "${NODE_DISTRO}"*; \
  node --version

# Install node-gyp
RUN npm install -g node-gyp

RUN mkdir -p /root/vscode
WORKDIR /root/vscode

ENV AS=/usr/bin/riscv64-linux-gnu-as \
  AR=/usr/bin/riscv64-linux-gnu-ar \
  CC=/usr/bin/riscv64-linux-gnu-gcc-10 \
  CPP=/usr/bin/riscv64-linux-gnu-cpp-10 \
  CXX=/usr/bin/riscv64-linux-gnu-g++-10 \
  LD=/usr/bin/riscv64-linux-gnu-ld \
  PKG_CONFIG_PATH=/usr/lib/riscv64-linux-gnu/pkgconfig
