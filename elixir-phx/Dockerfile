FROM bitwalker/alpine-elixir:1.11.0

RUN \
    apk update && \
    apk --no-cache add \
    curl bash openssh-client build-base make git ncurses yarn openssl-dev nmap \
    && rm -rf /tmp/*

RUN curl -sL https://sentry.io/get-cli/ | bash && sentry-cli -V

WORKDIR ${HOME}

CMD ["/bin/sh"]
