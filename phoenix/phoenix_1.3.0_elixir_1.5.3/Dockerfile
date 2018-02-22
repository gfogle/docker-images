FROM elixir:1.5.3

# replace shell with bash so we can source files
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# update the repository sources list and install dependencies
#
# - curl : we'll use this for alot such as nvm installs
# - inotify-tools: for phoenix file watching
RUN apt-get update \
    && apt-get install -y curl \
    && apt-get install -y inotify-tools \
    && apt-get -y autoclean

# install rebar
RUN mix local.rebar --force

# install hex
RUN mix local.hex --force

# install NVM and nodeJS
RUN curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash

# nvm environment vars
ENV NVM_DIR /root/.nvm
ENV NODE_VERSION 8.9.0

# install node and npm
RUN source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

# add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

# confirm installation
RUN node -v
RUN npm -v

# install phoenix
RUN mix archive.install --force  https://github.com/phoenixframework/archives/raw/master/phx_new-1.3.0.ez
