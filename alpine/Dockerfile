FROM alpine:3.16

LABEL maintainer="Jack + Joe <randy@jackjoe.be>"

RUN apk update
RUN apk --no-cache add openssl bash curl libstdc++ libc-dev libgcc ncurses-libs
RUN rm -rf /tmp/*

CMD ["/bin/sh"]
