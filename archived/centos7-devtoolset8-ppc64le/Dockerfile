FROM ppc64le/centos:centos7

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
    python3"
ARG NODE_VERSION="16.20.2"

# setup dependencies
RUN yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum -y clean all --enablerepo='*'

# setup nodejs repo
RUN curl -L -O https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-ppc64le.tar.gz && \
    tar -xzf node-v${NODE_VERSION}-linux-ppc64le.tar.gz && \
    cd node-v${NODE_VERSION}-linux-ppc64le && \
    cp -R * /usr/local/ && \
    cd .. && \
    rm -rf node-v${NODE_VERSION}-linux-ppc64le* && \
    node --version

# update npm and install yarn, node-gyp
RUN npm install -g yarn node-gyp

# jq (VSCodium)
RUN curl -L -O https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 && \
    chmod +x jq-linux64 && \
    mv jq-linux64 /usr/bin/jq

ENV PATH=/opt/rh/devtoolset-8/root/usr/bin/:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

RUN mkdir -p /root/vscode
WORKDIR /root/vscode
