[![Build Status](https://travis-ci.org/jackjoe/docker.svg?branch=master)](https://travis-ci.org/jackjoe/docker)

# docker

A collection of docker images we use.

### Laravel

Base image that has all dependencies needed for our Laravel projects. Contains only the dependencies, not Laravel itself.

_OS_: Alpine Linux

_Dependencies_:

* mcrypt
* soap
* yarn
* npm
* imagick

#### Flavours

PHP 7.1

`docker build -t jackjoe/laravel-php71 ./laravel-php71`

PHP 7.2

`docker build -t jackjoe/laravel-php72 ./laravel-php72`

### Gitlab CI Alpine

Vanilla Alpine, with a sent of openssh.

_OS_: Alpine Linux

_Dependencies_:

* openssh
* make
* bash

`docker build -t jackjoe/ci-deploy-alpine ./ci-deploy-alpine`

### Elixir + Phoenix

Container to build Phoenix apps

`docker build -t jackjoe/elixir-phx ./elixir-phx`
