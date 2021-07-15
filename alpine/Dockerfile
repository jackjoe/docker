FROM alpine:3.13.0

RUN apk update && \
    apk --no-cache add bash curl libstdc++ \
    && rm -rf /tmp/*

CMD ["/bin/sh"]
