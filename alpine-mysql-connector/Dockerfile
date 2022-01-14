FROM jackjoe/alpine:latest

LABEL maintainer="Jack + Joe <randy@jackjoe.be>"

RUN apk update
RUN apk add --no-cache mysql-client mariadb-connector-c
RUN rm -rf /tmp/*

CMD ["/bin/sh"]
