# wesnothd-1.14-16

Dockerized servers for The Battle for Wesnoth

## Description

Due to branches 1.14 and 1.16 share the same dependencies, the same Dockerfile can be used to generate images.

## Building images

By default, the image will be built using a fixed release of the branch.

``` bash
# This will build wesnothd v1.16.1
docker build -t wesnoth/wesnothd:1.16.1 wesnothd-1.14-16
```

You can especify other branch and version by passing the following arguments:

``` bash
# This will build wesnothd v1.14.10
docker build -t wesnoth/wesnothd:1.14.10 --build-arg WESNOTHD_BRANCH=1.14 --build-arg WESNOTHD_VERSION=1.14.10 --no-cache wesnothd-1.14-16
```
