FROM php:7.4-alpine3.13

LABEL maintainer="Jack + Joe <randy@jackjoe.be>" \
  php="7.4"

RUN apk add --no-cache \
      bash \
      ca-certificates \
      curl \
      file \
      git \
      make \
      openssh-client \
      openssl-dev \
      sudo \
      bzip2-dev \
      curl-dev \
      cyrus-sasl-dev \
      freetype-dev \
      icu-dev \
      imagemagick \
      imagemagick-dev \
      krb5-dev \
      libbz2 \
      libedit-dev \
      libintl \
      libzip \
      libjpeg-turbo-dev \
      libltdl \
      libpng-dev \
      libtool \
      libstdc++ \
      libxml2-dev \
      libxslt-dev \
      pcre-dev \
      sqlite-dev \
      zlib-dev \
      libzip-dev \
      && apk add --no-cache --virtual .build-deps \
      build-base  \
      autoconf \
      automake \
      libtool \
      && rm -rf /tmp/* \
      /usr/share/man

# do we need gd?
RUN docker-php-ext-install calendar \
      && docker-php-ext-configure bcmath \
      && docker-php-ext-install bcmath \
      && docker-php-ext-install bz2 \
      && docker-php-ext-install soap \
      && docker-php-ext-install intl \
      && docker-php-ext-install pdo_mysql \
      && docker-php-ext-install sockets \
      && docker-php-ext-install zip \
      && docker-php-ext-configure gd --with-jpeg=/usr/include/ --with-freetype=/usr/include/ \
      && docker-php-ext-install gd \
      \
      && set -ex \
      && apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS imagemagick-dev libtool \
      && export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS" \
      && pecl install imagick-3.4.3 \
      && docker-php-ext-enable imagick \
      && apk add --no-cache --virtual .imagick-runtime-deps imagemagick \
      && apk del .phpize-deps \
      \
      && pecl install ast \
      && docker-php-ext-enable ast \
      && docker-php-source delete

RUN echo "memory_limit=512M" > /usr/local/etc/php/conf.d/zz-conf.ini

# composer + hirak/prestissimo to speedup composer installs
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --2 --filename=composer \
      && composer global require hirak/prestissimo --prefer-dist --no-interaction \
      && rm -rf $HOME/.composer/cache/*

RUN apk add --no-cache libuv \
    && apk add --no-cache nodejs-current \
    && apk add --no-cache yarn nodejs-npm

# cleanup
RUN apk del --purge build-base .build-deps \
RUN rm -rf \
  /var/cache/apk/* \
  /tmp/* \
  /usr/share/man

CMD ["php", "--ini"]
