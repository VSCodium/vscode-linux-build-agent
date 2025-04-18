ARG REPO=mcr.microsoft.com/mirror/docker/library/ubuntu
ARG TAG=20.04
FROM ${REPO}:${TAG}

ARG DEBIAN_FRONTEND=noninteractive

# Disable user so it can run on GH (VSCodium)
# RUN groupadd --gid 1000 builduser \
#   && useradd --uid 1000 --gid builduser --shell /bin/bash --create-home builduser \

RUN mkdir -p /setup

# Set up TEMP directory
ENV TEMP=/tmp
RUN chmod a+rwx /tmp

# Avoid prompt for keyboard layout
RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# Latest stable git
RUN apt-get update && apt-get install -y software-properties-common
RUN add-apt-repository ppa:git-core/ppa -y

RUN apt-get update && apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    file \
    git \
    gnome-keyring \
    iproute2 \
    libfuse2 \
    libgconf-2-4 \
    libgdk-pixbuf2.0-0 \
    libgl1 \
    libgtk-3.0 \
    libsecret-1-dev \
    libkrb5-dev \
    libssl-dev \
    libx11-dev \
    libx11-xcb-dev \
    libxkbfile-dev \
    locales \
    lsb-release \
    lsof \
    python-dbus \
    python3-pip \
    sudo \
    wget \
    xvfb \
    tzdata \
    unzip \
    # VSCodium
    jq \
    imagemagick \
    gcc-10 \
    g++-10 \
  && curl https://chromium.googlesource.com/chromium/src/+/HEAD/build/install-build-deps.sh\?format\=TEXT | base64 --decode | cat > /setup/install-build-deps.sh \
  && curl https://chromium.googlesource.com/chromium/src/+/HEAD/build/install-build-deps.py\?format\=TEXT | base64 --decode | cat > /setup/install-build-deps.py \
  # Remove snapcraft to avoid issues on docker build
  && sed -i 's/packages.append("snapcraft")/#packages.append("snapcraft")/g' /setup/install-build-deps.py \
  && chmod +x /setup/install-build-deps.sh \
  && chmod +x /setup/install-build-deps.py \
  && bash /setup/install-build-deps.sh --no-syms --no-prompt --no-chromeos-fonts --no-arm --no-nacl \
  && rm -rf /var/lib/apt/lists/*

# Disable user so it can run on GH (VSCodium)
# # No Sudo Prompt
# RUN echo 'builduser ALL=NOPASSWD: ALL' >> /etc/sudoers.d/50-builduser \
#   && echo 'Defaults    env_keep += "DEBIAN_FRONTEND"' >> /etc/sudoers.d/env_keep

# Node.js
RUN curl --silent --location https://deb.nodesource.com/setup_20.x | sudo -E bash -
RUN apt-get update && apt-get install -y nodejs
RUN npm install -g npm@latest node-gyp yarn

# Set python3 as default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1
RUN python --version

# Install docker client
RUN sudo mkdir -m 0755 -p /etc/apt/keyrings
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
RUN echo "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN sudo apt-get update && sudo apt-get install -y docker-ce

# Set gcc-10 as default
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 10
RUN update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-10 10

# Check compiler toolchain
RUN gcc --version
RUN g++ --version

# Disable user so it can run on GH (VSCodium)
# USER builduser
# WORKDIR /home/builduser
