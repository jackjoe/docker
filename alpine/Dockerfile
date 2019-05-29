FROM alpine:3.9

RUN apk update && \
    apk --no-cache --update add bash curl \
      && rm  -rf /tmp/* /var/cache/apk/*

CMD ["/bin/sh"]
