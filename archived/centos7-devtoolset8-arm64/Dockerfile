FROM arm64v8/centos:centos7

ARG INSTALL_PKGS="devtoolset-8-gcc \
    devtoolset-8-gcc-c++ \
    devtoolset-8-gcc-gfortran \
    devtoolset-8-gdb \
    make \
    git \
    unzip \
    sudo \
    libsecret-devel \
    krb5-devel \
    nodejs \
    python3"

# setup nodejs repo
RUN curl --silent --location https://rpm.nodesource.com/setup_16.x | bash -

RUN yum install -y centos-release-scl-rh && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum -y clean all --enablerepo='*'

# update npm and install yarn, node-gyp
RUN npm install -g yarn node-gyp

# jq (VSCodium)
RUN curl -L -O https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 && \
    chmod +x jq-linux64 && \
    mv jq-linux64 /usr/bin/jq

ENV PATH=/opt/rh/devtoolset-8/root/usr/bin/:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

RUN mkdir -p /root/vscode
WORKDIR /root/vscode
