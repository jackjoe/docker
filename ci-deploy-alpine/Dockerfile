FROM jackjoe/alpine:latest

LABEL maintainer="Jack + Joe <randy@jackjoe.be>"

RUN apk --no-cache add \
      openssh make bash zip rsync git libstdc++ \
      && rm -rf /tmp/*

EXPOSE 22

CMD ["/bin/sh"]
