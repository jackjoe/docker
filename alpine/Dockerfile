FROM alpine:3.14

RUN apk update && \
    apk --no-cache add openssl bash curl libstdc++ libc-dev libgcc ncurses-libs \
    && rm -rf /tmp/*

CMD ["/bin/sh"]
