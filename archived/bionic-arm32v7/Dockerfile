FROM arm32v7/ubuntu:bionic

ARG DEBIAN_FRONTEND=noninteractive

# Disable user so it can run on GH (VSCodium)
# RUN groupadd --gid 1000 builduser \
#   && useradd --uid 1000 --gid builduser --shell /bin/bash --create-home builduser

RUN apt-get update && apt-get install -y --no-install-recommends \
  software-properties-common \
  gcc \
  g++ \
  binutils \
  build-essential \
  git \
  pkg-config \
  sudo \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg \
  make \
  python3 \
  libkrb5-dev \
  unzip \
  # VSCodium
  jq

# Set python3 as default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1
RUN python --version

# Disable user so it can run on GH (VSCodium)
# # No Sudo Prompt
# RUN echo 'builduser ALL=NOPASSWD: ALL' >> /etc/sudoers.d/50-builduser \
#   && echo 'Defaults    env_keep += "DEBIAN_FRONTEND"' >> /etc/sudoers.d/env_keep

# USER builduser
# WORKDIR /home/builduser
