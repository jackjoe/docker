FROM elixir:1.14-alpine

LABEL maintainer="Jack + Joe <randy@jackjoe.be>"

ENV MIX_HOME=/opt/mix \
    HEX_HOME=/opt/hex

RUN apk --no-cache --update upgrade && \
    apk add --no-cache --update --virtual .elixir-build make && \
    apk add --no-cache --update \
    curl bash openssh-client build-base coreutils htop \
    git ncurses yarn openssl-dev nmap libstdc++ libc-dev libgcc ncurses-libs

RUN mix local.hex --force && \
    mix local.rebar --force && \
    rm -rf /tmp/*

RUN curl -sL https://sentry.io/get-cli/ | bash && sentry-cli -V

WORKDIR /opt/app

CMD ["/bin/sh"]
