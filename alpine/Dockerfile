FROM alpine:3.14

LABEL maintainer="Jack + Joe <randy@jackjoe.be>"

RUN apk update && \
    apk --no-cache add openssl bash curl libstdc++ libc-dev libgcc ncurses-libs \
    && rm -rf /tmp/*

CMD ["/bin/sh"]
