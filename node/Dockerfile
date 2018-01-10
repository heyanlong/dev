FROM node:6

RUN set -ex \
    && apt-get update \
    && apt-get install python \
    && rm -rf /var/lib/apt/lists/*

RUN echo "registry=https://registry.npm.taobao.org" > /root/.npmrc

RUN set -ex \
    && npm i gulp -g