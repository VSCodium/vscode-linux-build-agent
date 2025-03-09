ARG REPO=linuxdeepin/deepin
ARG TAG=beige-amd64-v1.4.0
FROM ${REPO}:${TAG}

ARG DOCKER_CE_VERSION=27.3.1

# Disable user so it can run on GH (VSCodium)
# RUN groupadd --gid 1000 builduser \
#   && useradd --uid 1000 --gid builduser --shell /bin/bash --create-home builduser

ENV DEBIAN_FRONTEND=noninteractive
RUN dpkg --add-architecture loong64 && \
  echo 'deb [arch=amd64,i386,loong64] http://mirrors.kernel.org/deepin/beige beige main commercial community' > /etc/apt/sources.list && \
  apt-get update && \
  # --force-overwrite to work around a strange error:
  # dpkg: error processing archive /tmp/apt-dpkg-install-sG9gCG/20-linux-libc-dev_23.01.01.18_amd64.deb (--unpack):
  # trying to overwrite shared '/usr/share/doc/linux-libc-dev/changelog.gz', which is different from other instances of package linux-libc-dev:amd64
  apt-get install -y -f -o Dpkg::Options::="--force-overwrite" --no-install-recommends \
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
  CC=/usr/bin/loongarch64-linux-gnu-gcc-12 \
  CPP=/usr/bin/loongarch64-linux-gnu-cpp-12 \
  CXX=/usr/bin/loongarch64-linux-gnu-g++-12 \
  LD=/usr/bin/loongarch64-linux-gnu-ld \
  PKG_CONFIG_PATH=/usr/lib/loongarch64-linux-gnu/pkgconfig
