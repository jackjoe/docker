FROM elixir:1.6-alpine

RUN \
    apk update && \
    apk --no-cache --update add \
        bash openssh-client make git ncurses

RUN mix local.hex --force && \
    mix local.rebar --force
