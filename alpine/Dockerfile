FROM alpine:3.14.0

RUN apk update && \
    apk --no-cache add bash curl libstdc++ libc-dev libgcc ncurses-libs \
    && rm -rf /tmp/*

CMD ["/bin/sh"]
