FROM php:7.1-alpine

LABEL maintainer="Jack + Joe <randy@jackjoe.be>" \
  php="7.1"

# OS + tooling
RUN apk add --update --no-cache \
      bash \
      make \
      curl \
      git \
      grep \
      openssl-dev \
      openssh-client \
      sudo \
      file \
      bison \
      ca-certificates

RUN apk add --no-cache --virtual .build-deps build-base autoconf

######################################################
#             Installling Ruby                       #
######################################################
ENV RUBY_MAJOR 2.4
ENV RUBY_VERSION 2.4.1
ENV RUBY_DOWNLOAD_SHA256 4fc8a9992de3e90191de369270ea4b6c1b171b7941743614cc50822ddc1fe654
ENV RUBYGEMS_VERSION 2.6.12

# some of ruby's build scripts are written in ruby
#   we purge system ruby later to make sure our final image uses what we just built
# readline-dev vs libedit-dev: https://bugs.ruby-lang.org/issues/11869 and https://github.com/docker-library/ruby/issues/75
RUN set -ex \
  \
  && apk add --no-cache --virtual .ruby-builddeps \
    autoconf \
    bison \
    bzip2 \
    bzip2-dev \
    ca-certificates \
    coreutils \
    dpkg-dev dpkg \
    gcc \
    gdbm-dev \
    glib-dev \
    libc-dev \
    libffi-dev \
    libxml2-dev \
    libxslt-dev \
    linux-headers \
    make \
    ncurses-dev \
    openssl \
    openssl-dev \
    procps \
    readline-dev \
    ruby \
    tar \
    yaml-dev \
    zlib-dev \
    xz \
  \
  && wget -O ruby.tar.xz "https://cache.ruby-lang.org/pub/ruby/${RUBY_MAJOR%-rc}/ruby-$RUBY_VERSION.tar.xz" \
  && echo "$RUBY_DOWNLOAD_SHA256 *ruby.tar.xz" | sha256sum -c - \
  \
  && mkdir -p /usr/src/ruby \
  && tar -xJf ruby.tar.xz -C /usr/src/ruby --strip-components=1 \
  && rm ruby.tar.xz \
  \
  && cd /usr/src/ruby \
  \
# hack in "ENABLE_PATH_CHECK" disabling to suppress:
#   warning: Insecure world writable dir
  && { \
    echo '#define ENABLE_PATH_CHECK 0'; \
    echo; \
    cat file.c; \
  } > file.c.new \
  && mv file.c.new file.c \
  \
  && autoconf \
  && gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
# the configure script does not detect isnan/isinf as macros
  && export ac_cv_func_isnan=yes ac_cv_func_isinf=yes \
  && ./configure \
    --build="$gnuArch" \
    --disable-install-doc \
    --enable-shared \
  && make -j "$(nproc)" \
  && make install \
  \
  && runDeps="$( \
    scanelf --needed --nobanner --recursive /usr/local \
      | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
      | sort -u \
      | xargs -r apk info --installed \
      | sort -u \
  )" \
  && apk add --virtual .ruby-rundeps $runDeps \
    bzip2 \
    ca-certificates \
    libffi-dev \
    openssl-dev \
    yaml-dev \
    procps \
    zlib-dev \
  && apk del .ruby-builddeps \
  && cd / \
  && rm -r /usr/src/ruby \
  \
  && gem update --system "$RUBYGEMS_VERSION"

ENV BUNDLER_VERSION 1.15.3

RUN gem install bundler --version "$BUNDLER_VERSION"

# install things globally, for great justice
# and don't create ".bundle" in all our apps
ENV GEM_HOME /usr/local/bundle
ENV BUNDLE_PATH="$GEM_HOME" \
  BUNDLE_BIN="$GEM_HOME/bin" \
  BUNDLE_SILENCE_ROOT_WARNING=1 \
  BUNDLE_APP_CONFIG="$GEM_HOME"
ENV PATH $BUNDLE_BIN:$PATH
RUN mkdir -p "$GEM_HOME" "$BUNDLE_BIN" \
&& chmod 777 "$GEM_HOME" "$BUNDLE_BIN"

# php
RUN apk add --update --no-cache \
      zlib-dev \
      libbz2 \
      libxml2-dev \
      imap-dev \
      freetype-dev \
      libjpeg-turbo-dev \
      libltdl \
      libtool \
      libmcrypt-dev \
      libmcrypt \
      libedit-dev \
      libpng-dev \
      krb5-dev \
      icu-dev \
      readline-dev \
      bzip2-dev \
      curl-dev \
      libxslt-dev \
      imagemagick-dev \
      imagemagick \
      gmp-dev \
      postgresql-dev \
      libintl \
      pcre-dev \
      sqlite-dev \
      cyrus-sasl-dev

RUN docker-php-ext-configure bcmath \
      && docker-php-ext-configure bcmath \
      && docker-php-ext-configure bz2 \
      && docker-php-ext-configure calendar \
      && docker-php-ext-configure json \
      && docker-php-ext-configure mcrypt \
      && docker-php-ext-configure gmp \
      && docker-php-ext-configure iconv \
      && docker-php-ext-configure intl \
      && docker-php-ext-configure pcntl \
      && docker-php-ext-configure pdo \
      && docker-php-ext-configure pdo_mysql \
      && docker-php-ext-configure readline \
      && docker-php-ext-configure soap \
      && docker-php-ext-configure exif \
      && docker-php-ext-configure xml \
      && docker-php-ext-configure xmlrpc \
      && docker-php-ext-configure xsl \
      && docker-php-ext-configure zip \
      && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
      && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
      # && docker-php-ext-install -j$(getconf _NPROCESSORS_ONLN) $PHP_EXTENSIONS \
      # && docker-php-ext-install -j$(getconf _NPROCESSORS_ONLN) gd imap

      # && docker-php-ext-enable $PHP_EXTENSIONS \
RUN pecl install imagick \
      && docker-php-ext-enable imagick

RUN pecl install ast \
      && docker-php-ext-enable ast

RUN echo "memory_limit=512M" > /usr/local/etc/php/conf.d/zz-conf.ini

RUN cd /
RUN docker-php-source delete

# composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
# Install hirak/prestissimo to speedup composer installs.
RUN composer global require hirak/prestissimo --prefer-dist --no-interaction
RUN rm -rf $HOME/.composer/cache/*

RUN echo -e 'http://dl-cdn.alpinelinux.org/alpine/edge/main\nhttp://dl-cdn.alpinelinux.org/alpine/edge/community\nhttp://dl-cdn.alpinelinux.org/alpine/edge/testing' > /etc/apk/repositories \
      && apk add --update nodejs nodejs-npm yarn
RUN npm install -g npm

# cleanup
RUN apk del --purge grep build-base .build-deps
RUN rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/include/php7 /usr/lib/php7/build /usr/lib/php7/modules/*.a
