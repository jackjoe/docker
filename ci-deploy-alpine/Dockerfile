FROM jackjoe/alpine

LABEL maintainer="Jack + Joe <randy@jackjoe.be>"

RUN apk add --update \
      openssh \
      make \
      bash \
      zip \
      rsync \
      git \
      && rm  -rf /tmp/* /var/cache/apk/*

EXPOSE 22

CMD ["/bin/sh"]
