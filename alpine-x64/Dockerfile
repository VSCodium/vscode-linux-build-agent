ARG REPO=alpine
ARG TAG=3.16
FROM ${REPO}:${TAG}

ARG NODE_VERSION="22.20.0"

RUN apk add g++ python3 make git bash curl perl pkgconfig libsecret-dev krb5-dev jq

# setup nodejs repo
RUN curl -L -O https://unofficial-builds.nodejs.org/download/release/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64-musl.tar.gz && \
    tar -xzf node-v${NODE_VERSION}-linux-x64-musl.tar.gz && \
    cd node-v${NODE_VERSION}-linux-x64-musl && \
    cp -R * /usr/local/ && \
    cd .. && \
    rm -rf node-v${NODE_VERSION}-linux-x64-musl* && \
    node --version

# install yarn, node-gyp
RUN npm install -g yarn node-gyp

RUN mkdir -p /root/vscode
WORKDIR /root/vscode
