ARG REPO=mcr.microsoft.com/mirror/docker/library/ubuntu
ARG TAG=20.04
FROM ${REPO}:${TAG}

ARG DEBIAN_FRONTEND=noninteractive

# ubuntu keeps its i386/amd64 and other architecture repos separate
# the default i386/amd64 packages can be found in http://archive.ubuntu.com while all other architectures are in http://ports.ubuntu.com/
# add the required repos for ppc64el and only fetch i386/amd64 from archive.ubuntu.com (to prevent apt from erroring about missing architecture support)
RUN sed -i "s/^deb/deb [arch=amd64,i386]/g" /etc/apt/sources.list
RUN echo "deb [arch=ppc64el] http://ports.ubuntu.com/ focal main universe multiverse restricted" | tee -a /etc/apt/sources.list
RUN echo "deb [arch=ppc64el] http://ports.ubuntu.com/ focal-security main universe multiverse restricted" | tee -a /etc/apt/sources.list
RUN echo "deb [arch=ppc64el] http://ports.ubuntu.com/ focal-updates main universe multiverse restricted" | tee -a /etc/apt/sources.list
RUN dpkg --add-architecture ppc64el
RUN apt-get update && apt-get install -y --no-install-recommends \
  gcc-8-powerpc64le-linux-gnu \
  g++-8-powerpc64le-linux-gnu \
  binutils-powerpc64le-linux-gnu \
  build-essential \
  git \
  pkg-config \
  fakeroot \
  rpm \
  sudo \
  apt-transport-https \
  ca-certificates \
  libx11-dev:ppc64el \
  libx11-xcb-dev:ppc64el \
  libxkbfile-dev:ppc64el \
  libsecret-1-dev:ppc64el \
  libkrb5-dev:ppc64el \
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
RUN curl --silent --location https://deb.nodesource.com/setup_18.x | sudo -E bash -
RUN apt-get update && apt-get install -y nodejs
RUN npm install -g npm@latest
RUN npm install -g node-gyp

ENV AS=/usr/bin/powerpc64le-linux-gnu-as \
  AR=/usr/bin/powerpc64le-linux-gnu-ar \
  CC=/usr/bin/powerpc64le-linux-gnu-gcc-8 \
  CPP=/usr/bin/powerpc64le-linux-gnu-cpp-8 \
  CXX=/usr/bin/powerpc64le-linux-gnu-g++-8 \
  LD=/usr/bin/powerpc64le-linux-gnu-ld \
  PKG_CONFIG_PATH=/usr/lib/powerpc64le-linux-gnu/pkgconfig
