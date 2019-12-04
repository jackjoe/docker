FROM docker:19.03.4

LABEL maintainer="Jack + Joe <randy@jackjoe.be>"

RUN apk add --update \
      make \
      bash \
      git \
      && rm  -rf /tmp/* /var/cache/apk/*

CMD ["/bin/sh"]
