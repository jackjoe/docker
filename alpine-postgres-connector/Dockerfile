FROM jackjoe/alpine:latest

LABEL maintainer="Jack + Joe <randy@jackjoe.be>"

RUN apk update
RUN echo 'http://dl-cdn.alpinelinux.org/alpine/v3.14/main' >> /etc/apk/repositories
RUN apk add --no-cache postgresql-client=13.8-r0
RUN rm -rf /tmp/*

CMD ["/bin/sh"]
