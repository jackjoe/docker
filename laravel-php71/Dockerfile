FROM php:7.1-alpine

LABEL maintainer="Jack + Joe <randy@jackjoe.be>" \
  php="7.1"

RUN apk add --update --no-cache \
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
      libjpeg-turbo-dev \
      libltdl \
      libmcrypt \
      libmcrypt-dev \
      libpng-dev \
      libxml2-dev \
      libxslt-dev \
      pcre-dev \
      sqlite-dev \
      zlib-dev \
      && apk add --no-cache --virtual .build-deps \
      build-base \
      autoconf \
      libtool \
      && rm -rf /var/cache/apk/* \
      /tmp/* \
      /usr/share/man

# do we need gd?
RUN docker-php-ext-configure bcmath \
      && docker-php-ext-install calendar \
      && docker-php-ext-install bcmath \
      && docker-php-ext-install bz2 \
      && docker-php-ext-install soap \
      && docker-php-ext-install intl \
      && docker-php-ext-install mcrypt \
      && docker-php-ext-install pdo_mysql \
      && docker-php-ext-install zip \
      && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
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
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
      && composer global require hirak/prestissimo --prefer-dist --no-interaction \
      && rm -rf $HOME/.composer/cache/*

RUN echo -e 'http://dl-cdn.alpinelinux.org/alpine/edge/main\nhttp://dl-cdn.alpinelinux.org/alpine/edge/community\nhttp://dl-cdn.alpinelinux.org/alpine/edge/testing' > /etc/apk/repositories \
      && apk add --update nodejs nodejs-npm yarn \
      && npm install -g npm

# RUN git clone https://github.com/sass/sassc.git \
#       && git clone https://github.com/sass/libsass.git \
#       && cd libsass \
#       && autoreconf --force --install \
#       && cd .. \
#       && cd libsass \
#       && ./configure \
#       --disable-tests \
#       --enable-shared \
#       --prefix=/usr \
#       && cd .. \
#       && make -C libsass -j4 \
#       && make -C libsass install \
#       && cd sassc \
#       && autoreconf --force --install \
#       && cd .. \
#       && cd sassc \
#       && ./configure \
#       --enable-shared \
#       --prefix=/usr \
#       && cd .. \
#       && make -C sassc -j4 \
#       && make -C sassc install \
#       && sassc --version \
#       && rm -rf ./libsass ./sassc

# cleanup
RUN apk del --purge grep build-base .build-deps \
  && rm -rf \
  /var/cache/apk/* \
  /tmp/* \
  /usr/share/man

CMD ["php", "--ini"]
