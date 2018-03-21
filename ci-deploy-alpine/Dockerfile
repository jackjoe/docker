FROM alpine

LABEL maintainer="Jack + Joe <randy@jackjoe.be>"

RUN apk add --update \
      openssh \
      make \
      bash \
      && rm  -rf /tmp/* /var/cache/apk/*

EXPOSE 22

CMD ["/bin/sh"]
