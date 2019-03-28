FROM elixir:1.8

# https://pkgs.alpinelinux.org/package/edge/main/ppc64le/build-base

RUN \
    apk update && \
    apk --no-cache --update add \
        bash openssh-client build-base git ncurses yarn

RUN mix local.hex --force && \
    mix local.rebar --force
