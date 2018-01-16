FROM php:7.1-alpine

LABEL maintainer="Jack + Joe <randy@jackjoe.be>" \
  php="7.1"

RUN apk add --update --no-cache \
      bash \
      bison \
      ca-certificates \
      curl \
      file \
      git \
      grep \
      make \
      openssh-client \
      openssl-dev \
      sudo \
      && apk add --no-cache --virtual \
      .build-deps \
      build-base \
      autoconf \
      autotools-dev \
      libtool

RUN apk add --update --no-cache \
      bzip2-dev \
      curl-dev \
      cyrus-sasl-dev \
      freetype-dev \
      gmp-dev \
      icu-dev \
      imagemagick \
      imagemagick-dev \
      imap-dev \
      krb5-dev \
      libbz2 \
      libedit-dev \
      libintl \
      libjpeg-turbo-dev \
      libltdl \
      libmcrypt \
      libmcrypt-dev \
      libpng-dev \
      libtool \
      libxml2-dev \
      libxslt-dev \
      pcre-dev \
      postgresql-dev \
      readline-dev \
      sqlite-dev \
      zlib-dev

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
      && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
      \
      && pecl install imagick \
      && docker-php-ext-enable imagick
      && pecl install ast \
      && docker-php-ext-enable ast \
      && docker-php-source delete

RUN echo "memory_limit=512M" > /usr/local/etc/php/conf.d/zz-conf.ini

# composer + hirak/prestissimo to speedup composer installs
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
      && composer global require hirak/prestissimo --prefer-dist --no-interaction \
      && rm -rf $HOME/.composer/cache/*

RUN echo -e 'http://dl-cdn.alpinelinux.org/alpine/edge/main\nhttp://dl-cdn.alpinelinux.org/alpine/edge/community\nhttp://dl-cdn.alpinelinux.org/alpine/edge/testing' > /etc/apk/repositories \
      && apk add --update nodejs nodejs-npm yarn
      && npm install -g npm

RUN git clone https://github.com/sass/sassc.git \
      && git clone https://github.com/sass/libsass.git \
      && cd libsass \
      && autoreconf --force --install \
      && cd .. \
      && cd libsass \
      && ./configure \
      &&   --disable-tests \
      &&   --enable-shared \
      &&   --prefix=/usr \
      && cd .. \
      && make -C libsass -j4 \
      && make -C libsass install \
      && cd sassc \
      && autoreconf --force --install \
      && cd .. \
      && cd sassc \
      && ./configure \
      &&   --enable-shared \
      &&   --prefix=/usr \
      && cd .. \
      && make -C sassc -j4 \
      && make -C sassc install \
      && sassc --version
      && rm -rf ./libsass ./sassc

# cleanup
RUN apk del --purge grep build-base .build-deps
RUN rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/include/php7 /usr/lib/php7/build /usr/lib/php7/modules/*.a
