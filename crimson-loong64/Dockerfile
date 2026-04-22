ARG REPO=linuxdeepin/deepin
ARG TAG=crimson
FROM ${REPO}:${TAG}

ARG DOCKER_CE_VERSION=27.3.1

# Disable user so it can run on GH (VSCodium)
# RUN groupadd --gid 1000 builduser \
#   && useradd --uid 1000 --gid builduser --shell /bin/bash --create-home builduser

ENV DEBIAN_FRONTEND=noninteractive
RUN echo 'deb [arch=amd64,i386,loong64] https://community-packages.deepin.com/beige crimson main commercial community' > /etc/apt/sources.list
RUN dpkg --add-architecture loong64
RUN apt-get update

# Work around Multi-Arch: same version skew between linux-libc-dev:amd64
# (25.01.01.20) and :loong64 (25.01.01.21) in the crimson index. Pin both
# architectures to a matching pair from the pool and hold them.
RUN curl -fL -o /tmp/linux-libc-dev_amd64.deb \
    https://community-packages.deepin.com/beige/pool/main/l/linux-upstream/linux-libc-dev_25.01.01.05_amd64.deb && \
  curl -fL -o /tmp/linux-libc-dev_loong64.deb \
    https://community-packages.deepin.com/beige/pool/main/l/linux-upstream/linux-libc-dev_25.01.01.05_loong64.deb && \
  dpkg -i --force-overwrite /tmp/linux-libc-dev_amd64.deb /tmp/linux-libc-dev_loong64.deb && \
  apt-mark hold linux-libc-dev && \
  rm /tmp/linux-libc-dev_*.deb

RUN apt-get install -y -f -o Dpkg::Options::="--force-overwrite" --no-install-recommends \
    gcc-12-loongarch64-linux-gnu \
    g++-12-loongarch64-linux-gnu \
    binutils-loongarch64-linux-gnu \
    build-essential \
    git \
    pkg-config \
    fakeroot \
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
RUN curl --silent --location https://deb.nodesource.com/setup_22.x | sudo -E bash - && \
  apt-get update && \
  apt-get install -y nodejs && \
  npm install -g npm@10 node-gyp yarn

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
  CC=/usr/bin/loongarch64-linux-gnu-gcc-12 \
  CPP=/usr/bin/loongarch64-linux-gnu-cpp-12 \
  CXX=/usr/bin/loongarch64-linux-gnu-g++-12 \
  LD=/usr/bin/loongarch64-linux-gnu-ld \
  PKG_CONFIG_PATH=/usr/lib/loongarch64-linux-gnu/pkgconfig
