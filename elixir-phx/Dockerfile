FROM bitwalker/alpine-elixir:1.8.1

RUN \
    apk update && \
    apk --no-cache --update add \
    curl bash openssh-client build-base git ncurses yarn openssl-dev

RUN curl -sL https://sentry.io/get-cli/ | bash && sentry-cli -V

WORKDIR ${HOME}

CMD ["/bin/sh"]
