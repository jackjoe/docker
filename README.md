# docker

A collection of docker images we use.

### Laravel php71

[![Build Status](https://travis-ci.org/jackjoe/docker.svg?branch=master)](https://travis-ci.org/jackjoe/docker)

Base image that has all dependencies needed for our Laravel projects. Contains only the dependencies, not Laravel itself.

_OS_: Alpine Linux

_Dependencies_:

* mcrypt
* soap
* yarn
* npm
* imagick
* sassc (libsass)

`docker build -t jackjoe/laravel:php71 ./laravel-php71`
