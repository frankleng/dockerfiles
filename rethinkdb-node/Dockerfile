FROM debian:buster-slim

SHELL ["/bin/bash", "--login", "-c"]

RUN apt-get -qqy update \
    && apt-get install -y --no-install-recommends ca-certificates gnupg2 curl \
    && rm -rf /var/lib/apt/lists/*

RUN apt-key adv --keyserver keys.gnupg.net --recv-keys "539A3A8C6692E6E3F69B3FE81D85E93F801BB43F" \
    && echo "deb https://download.rethinkdb.com/repository/debian-buster buster main" > /etc/apt/sources.list.d/rethinkdb.list

ENV RETHINKDB_PACKAGE_VERSION 2.4.1~0buster

RUN apt-get -qqy update \
	&& apt-get install -y rethinkdb=$RETHINKDB_PACKAGE_VERSION \
	&& apt-get -y autoclean \
	&& rm -rf /var/lib/apt/lists/*

ENV NODE_VERSION=16
ENV NVM_DIR=/usr/local/nvm

RUN mkdir -p /usr/local/nvm/ \
    && curl --silent -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash \
    && source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default
ENV NODE_PATH $NVM_DIR/versions/node/$NODE_VERSION/lib/node_modules
ENV PATH      $NVM_DIR/versions/node/$NODE_VERSION/bin:$PATH
RUN node --version
RUN npm --version

VOLUME ["/data"]

WORKDIR /data

CMD ["rethinkdb", "--bind", "all"]

#   process cluster webui
EXPOSE 28015 29015 8080
