# wesnothd-docker

Dockerized servers for The Battle for Wesnoth

## Description

This project is aimed to bring an easy an clean way to deploy game servers for any version of The Battle for Wesnoth, with independence of your execution environment.

### Why you should use this?

#### For nostalgic players

As the time passes, it gets more difficult to run earlier versions of The Battle for Wesnoth. Maybe you liked version 1.12 more than the current stable, but unless you have an older OS, you won't be able to run it.

Of course, if you just want to have the client for playing, there are alternatives (i.e.: Wine, Windows, VM, etc.). But what if you want to host a game in your own separate server?

#### The right choice for hosting games

In the most of the cases, your host could have not access to a concrete version of the game. Maybe it got stuck in version 1.14 and you want to host games for version 1.16. Definitely, running a containerized server will save you from dealing with dependency hell and the waste of resources.

#### Small and reusable

The binaries are built from sources. The resulting images don't occupy more than 165mb. Use them when needed, customize and reuse them, and remove cleanly when you want.

## Getting Started

### Dependencies

- Docker >= 20.10.11

### Building images

By default, the image will be built using a fixed release of the branch.

``` bash
# This will build wesnothd v1.12.6
docker build -t wesnoth/wesnothd:1.12.6 wesnothd-1.12
```

You can especify other version by passing the following arguments:

``` bash
# This will build wesnothd v1.12.4
docker build -t wesnoth/wesnothd:1.12.4 --build-arg WESNOTHD_VERSION=1.12.4 --no-cache wesnothd-1.12
```

Additional instructions may be provided for building specific versions. See corresponding README for each case.

### Running containers

By default, the server listens to port 15000/tcp. The following is the most basic command for running the container:

``` bash
# This will run wesnothd v1.16.1 in background listening on port 15000
docker run -d -p 15000:15000 --name wesnothd-1.16.1 wesnoth/wesnothd:1.16.1
```

You may prefer to use a different port:

``` bash
# This will specify custom ports for host and container
docker run -d -p 3000:7000 --name wesnothd-1.16.1 wesnoth/wesnothd:1.16.1 bin/wesnothd -p 7000
```

You could also take advantage of data persistence using a bind mount for reusing config files:

``` bash
# This will use a custom config that won't dissapear when container stops
docker run -d -v /path/to/config:/home/wesnothd/config -p 15000:15000 --name wesnothd-1.16.1 wesnoth/wesnothd:1.16.1 bin/wesnothd -c ../config/server.cfg
```

## Contributing

Any help is always welcomed. Just send PR or contact me.

## Authors

- @joseruizdev <joseruiz@keemail.me>

## License

Licensed under GPLv3.
