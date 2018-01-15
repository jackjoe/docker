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

# yarn/npm
RUN echo -e 'http://dl-cdn.alpinelinux.org/alpine/edge/main\nhttp://dl-cdn.alpinelinux.org/alpine/edge/community\nhttp://dl-cdn.alpinelinux.org/alpine/edge/testing' > /etc/apk/repositories \
      && apk add --update nodejs nodejs-npm yarn
RUN npm install -g npm

# cleanup
RUN apk del --purge grep build-base .build-deps
RUN rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/include/php7 /usr/lib/php7/build /usr/lib/php7/modules/*.a
