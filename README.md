[![Build Status](https://travis-ci.org/jackjoe/docker.svg?branch=master)](https://travis-ci.org/jackjoe/docker)

# Docker

A collection of docker images we use.

## Laravel

Base image that has all dependencies needed for our Laravel projects. Contains only the dependencies, not Laravel itself.

_Based on_: `php:7.x-alpine`

_OS_: Alpine Linux

_Packages_:

- mcrypt
- soap
- yarn
- npm
- imagick
- composer

### PHP 7.1

`docker build -t jackjoe/laravel-php71 ./laravel-php71`

### PHP 7.2

`docker build -t jackjoe/laravel-php72 ./laravel-php72`

### PHP 7.3

`docker build -t jackjoe/laravel-php73 ./laravel-php73`

### PHP 7.4

`docker build -t jackjoe/laravel-php74 ./laravel-php74`

## Alpine

Vanilla Alpine, with some `bash`.

_Based on_: `alpine:3.10.3`

_OS_: Alpine Linux

_Packages_:

- curl
- bash

`docker build -t jackjoe/alpine ./alpine`

## Gitlab CI Alpine

Vanilla Alpine, with some `bash`.

_Based on_: `jackjoe/alpine`

_OS_: Alpine Linux

_Packages_:

- bash
- openssh
- make
- bash
- zip
- rsync
- git

`docker build -t jackjoe/alpine ./alpine`

## Elixir + Phoenix

Container to build Phoenix apps, based on Bitwalkers image.

_Based on_: `bitwalker/alpine-elixir:1.10.2`

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

## FPM + nginx

Container with nginx and fpm in one container.

_Based on_: `php:7.3-fpm-alpine`

- root directory: /var/www/html/public/
- site config: /etc/nginx/conf.d/default.conf
- nginx config: /etc/nginx/nginx.conf

_OS_: Alpine Linux

_Packages_:

- nginx 1.17.6
- openssh-client
- build-base
- git
- ncurses
- yarn
- openssl-dev
- bash
- curl
- libpng-dev
- libjpeg-turbo-dev

_PHP Packages_:

- composer
- zip
- gd
- bcmath
- mb_string
- pdf_mysql
- mysqli
- intl
- bz2
- calendar

`docker build -t jackjoe/fpm-nginx ./fpm-nginx`

## Docker

Our own Docker, to use as base builder in Gitlab. Extends from the base docker image with make, bash and git.
