FROM node:18-bullseye AS Builder
# WAS ENV CRONICLE_VERSION=0.9.46
ENV CRONICLE_VERSION=0.9.59
WORKDIR /opt/cronicle
RUN curl -L -o /tmp/Cronicle-${CRONICLE_VERSION}.tar.gz https://github.com/jhuckaby/Cronicle/archive/refs/tags/v${CRONICLE_VERSION}.tar.gz
# COPY Cronicle-${CRONICLE_VERSION}.tar.gz /tmp/
RUN tar zxvf /tmp/Cronicle-${CRONICLE_VERSION}.tar.gz -C /tmp/ && \
    mv /tmp/Cronicle-${CRONICLE_VERSION}/* . && \
    rm -rf /tmp/* && \
    yarn
COPY ./patches /tmp/patches
RUN patch -p3 < /tmp/patches/engine.patch lib/engine.js
COPY docker-entrypoint.js ./bin/

FROM node:18-alpine
RUN apk add procps curl

# Add Python and packages
RUN apk add --update --no-cache python3 py3-pip
RUN pip install mysql-connector-python --break-system-packages
# Add mysql
RUN apk add mysql-client

COPY --from=builder /opt/cronicle/ /opt/cronicle/
WORKDIR /opt/cronicle
ENV CRONICLE_foreground=1
ENV CRONICLE_echo=1
ENV CRONICLE_color=1
ENV debug_level=1
ENV HOSTNAME=main
RUN node bin/build.js dist && bin/control.sh setup
CMD ["node", "bin/docker-entrypoint.js"]
