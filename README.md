[![Build Status](https://travis-ci.org/jackjoe/docker.svg?branch=master)](https://travis-ci.org/jackjoe/docker)

# Docker

A collection of docker images we use.

### Laravel

Base image that has all dependencies needed for our Laravel projects. Contains only the dependencies, not Laravel itself.

_OS_: Alpine Linux

_Packages_:

- mcrypt
- soap
- yarn
- npm
- imagick

#### Flavours

PHP 7.1

`docker build -t jackjoe/laravel-php71 ./laravel-php71`

PHP 7.2

`docker build -t jackjoe/laravel-php72 ./laravel-php72`

PHP 7.3

`docker build -t jackjoe/laravel-php73 ./laravel-php73`

### Gitlab CI Alpine

Vanilla Alpine, with some `bash`.

_OS_: Alpine Linux

_Packages_:

- bash

`docker build -t jackjoe/alpine ./alpine`

### Elixir + Phoenix

Container to build Phoenix apps, based on Bitwalkers image.

_OS_: Alpine Linux

_Packages_:

- openssh-client
- build-base
- git
- ncurses
- yarn
- openssl-dev
- bash
- curl

`docker build -t jackjoe/elixir-phx ./elixir-phx`

### FPM + nginx

Container with nginx and fpm in one container.

_OS_: Alpine Linux

_Packages_:

- ...

`docker build -t jackjoe/fpm-nginx ./fpm-nginx
