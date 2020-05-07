FROM node:8-alpine

RUN apk add --update --no-cache git \
                                libzmq \
                                zeromq-dev \
                                python \
                                make \
                                g++

WORKDIR /insight

# Copy bitgreen-node
RUN git clone --branch master --single-branch --depth 1 https://github.com/bitgreen/bitgreen-node.git .

# Copy config file
COPY bitgreen-node.json .

# Install NPM modules
RUN npm ci

ARG VERSION
ARG MAJOR_VERSION

# Install Insight modules
RUN bin/bitgreen-node install bitgreen/insight-api@${MAJOR_VERSION}
RUN bin/bitgreen-node install bitgreen/insight-ui@${VERSION}

FROM node:8-alpine

LABEL maintainer="Dash Developers <dev@dash.org>"
LABEL description="Dockerised Insight-Dash"

WORKDIR /insight

# Copy project files
COPY --from=0 /insight/ .

EXPOSE 3001

CMD ["bin/bitgreen-node", "start"]
