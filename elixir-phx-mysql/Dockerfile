FROM bitwalker/alpine-elixir:1.10.2

RUN \
    apk update && \
    apk --no-cache add \
    curl bash openssh-client build-base make git ncurses yarn openssl-dev \
    && rm -rf /tmp/*

RUN curl -sL https://sentry.io/get-cli/ | bash && sentry-cli -V

WORKDIR ${HOME}

# mariadb start
RUN apk add --no-cache openrc mariadb mariadb-client mariadb-server-utils pwgen && \
    rm -f /var/cache/apk/*

ADD files/run.sh /scripts/run.sh
RUN mkdir /docker-entrypoint-initdb.d && \
      mkdir /scripts/pre-exec.d && \
      mkdir /scripts/pre-init.d && \
      chmod -R 755 /scripts

EXPOSE 3306

VOLUME ["/var/lib/mysql"]

WORKDIR ${HOME}

ENTRYPOINT ["/scripts/run.sh"]

# mariadb stop

# CMD ["/bin/sh"]
