FROM bitwalker/alpine-elixir:1.8.1

# https://pkgs.alpinelinux.org/package/edge/main/ppc64le/build-base

RUN \
    apk update && \
    apk --no-cache --update add \
    curl bash openssh-client build-base git ncurses yarn

RUN curl -sL https://sentry.io/get-cli/ | bash

WORKDIR ${HOME}

CMD ["/bin/sh"]
