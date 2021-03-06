Fluentd Docker Image
====================

[![Build Status](https://travis-ci.org/fluent/fluentd-docker-image.svg?branch=master)](https://travis-ci.org/fluent/fluentd-docker-image)
[![Docker Stars](https://img.shields.io/docker/stars/fluent/fluentd.svg)](https://hub.docker.com/r/fluent/fluentd)
[![Docker Pulls](https://img.shields.io/docker/pulls/fluent/fluentd.svg)](https://hub.docker.com/r/fluent/fluentd)
[![ImageLayers Size](https://img.shields.io/imagelayers/image-size/fluent/fluentd/latest.svg)](https://hub.docker.com/r/fluent/fluentd)
[![ImageLayers Layers](https://img.shields.io/imagelayers/layers/fluent/fluentd/latest.svg)](https://hub.docker.com/r/fluent/fluentd)

## What is Fluentd?

Fluentd is an open source data collector, which lets you unify the data
collection and consumption for a better use and understanding of data.

> [www.fluentd.org](https://www.fluentd.org/)

![Fluentd Logo](https://www.fluentd.org/assets/img/miscellany/fluentd-logo.png)

## Supported tags and respective `Dockerfile` links

### Current images

These tags have image version postfix. This updates many places so we need feedback for improve/fix the images.

- `v1.4.0-1.0`, `v1.4-1`, `edge`
  [(v1.4/alpine/Dockerfile)][113]
- `v1.4.0-onbuild-1.0`, `v1.4-onbuild-1`, `edge-onbuild`
  [(v1.4/alpine-onbuild/Dockerfile)][114]
- `v1.4.0-debian-1.0`, `v1.4-debian-1`, `edge-debian`
  [(v1.4/debian/Dockerfile)][115]
- `v1.4.0-debian-onbuild-1.0`, `v1.4-debian-onbuild-1`, `edge-debian-onbuild`
  [(v1.4/debian-onbuild/Dockerfile)][116]
- `v0.12.43-1.0`, `v0.12-1`
  [(v0.12/alpine/Dockerfile)][101]
- `v0.12.43-onbuild-1.0`, `v0.12-onbuild-1`
  [(v0.12/alpine-onbuild/Dockerfile)][102]
- `v0.12.43-debian-1.0`, `v0.12-debian-1`
  [(v0.12/debian/Dockerfile)][105]
- `v0.12.43-debian-onbuild-1.0`, `v0.12-debian-onbuild-1`
  [(v0.12/debian-onbuild/Dockerfile)][106]

### Older images (before official image)

- `v1.3.2`, `v1.3`, `stable`, `latest`
  [(v1.3/alpine/Dockerfile)][113]
- `v1.3.2-onbuild`, `v1.3-onbuild`, `stable-onbuild`, `onbuild`
  [(v1.3/alpine-onbuild/Dockerfile)][114]
- `v1.3.2-debian`, `v1.3-debian`, `stable-debian`, `debian`
  [(v1.3/debian/Dockerfile)][115]
- `v1.3.2-debian-onbuild`, `v1.3-debian-onbuild`, `stable-debian-onbuild`,
  [(v1.3/debian-onbuild/Dockerfile)][116]
- `v0.12.43`, `v0.12`
  [(v0.12/alpine/Dockerfile)][101]
- `v0.12.43-onbuild`, `v0.12-onbuild`
  [(v0.12/alpine-onbuild/Dockerfile)][102]
- `v0.12.43-debian`, `v0.12-debian`
  [(v0.12/debian/Dockerfile)][105]
- `v0.12.43-debian-onbuild`, `v0.12-debian-onbuild`
  [(v0.12/debian-onbuild/Dockerfile)][106]

We recommend to use debian version for production because it uses jemalloc to mitigate memory fragmentation issue.

v1.x is for fluentd v1.x releases. This is current stable version.
v0.12 is for fluentd v0.12.x releases. This is old stable.

You can use older versions via tag. See [tag page on Docker Hub](https://hub.docker.com/r/fluent/fluentd/tags/).

### Using Kubernetes?

Check [fluentd-kubernetes-daemonset](https://github.com/fluent/fluentd-kubernetes-daemonset) images.

## The detail of image tag

This image is based on the popular [Alpine Linux project][1], available in
[the alpine official image][2], and Debian images.

### For current images

#### `edge`

Latest version of edge Fluentd branch (currently `v1.3-1`).

#### `vX.Y-A`

Latest version of `vX.Y` Fluentd branch.

`A` will be incremented when image has major changes.

#### `vX.Y.Z-A.B`

Concrete `vX.Y.Z` version of Fluentd.

`A` will be incremented when image has major changes.
`B` will be incremented when image has small changes, e.g. library update or bug fixes.

#### `onbuild` included tag

This image makes building derivative images easier.
See ["How to build your own image"](#how-to-build-your-own-image) section for more details.

#### `debian` included tag

The image based on [Debian Linux image][7].
You may use this image when you require plugins which cannot be installed on Alpine (like `fluent-plugin-systemd`).

#### `armhf` included tag

The `armhf` images use ARM base images for use on devices such as Raspberry Pis.

Furthermore, the base images enable support for cross-platform builds using the cross-build tools from [resin.io](https://docs.resin.io/reference/base-images/resin-base-images/#resin-xbuild-qemu).

In order to build these images natively on ARM devices, the `CROSS_BUILD_START` and `CROSS_BUILD_END` Docker build arguments must be set to the shell no-op (`:`), for example:
```bash
docker build --build-arg CROSS_BUILD_START=":" --build-arg CROSS_BUILD_END=":" -t fluent/fluentd:v1.3.2-onbuild v1.3/armhf/alpine-onbuild
```
(assuming the command is run from the root of this repository).

### For older images

#### `stable`, `latest`

Latest version of stable Fluentd branch (currently `v1.3`).

#### `vX.Y`

Latest version of `vX.Y` Fluentd branch.

#### `vX.Y.Z`

Concrete `vX.Y.Z` version of Fluentd.

#### `onbuild` included tag, `debian` included tag, `armhf` included tag

Same as current images.

## How to use this image

To create endpoint that collects logs on your host just run:

```bash
docker run -d -p 24224:24224 -p 24224:24224/udp -v /data:/fluentd/log fluent/fluentd:v1.3-debian-1
```

Default configurations are to:

- listen port `24224` for Fluentd forward protocol
- store logs with tag `docker.**` into `/fluentd/log/docker.*.log`
  (and symlink `docker.log`)
- store all other logs into `/fluentd/log/data.*.log` (and symlink `data.log`)

## Providing your own configuration file and additional options

`fluentd` arguments can be appended to the `docker run` line

For example, to provide a bespoke config and make `fluentd` verbose, then:

`docker run -ti --rm -v /path/to/dir:/fluentd/etc fluentd -c /fluentd/etc/<conf> -v`

The first `-v` tells Docker to share '/path/to/dir' as a volume and mount it at /fluentd/etc
The `-c` after the container name (fluentd) tells `fluentd` where to find the config file
The second `-v` is passed to `fluentd` to tell it to be verbose

## Change running user

Use `-u` option with `docker run`.

`docker run -p 24224:24224 -u foo -v ...`

## How to build your own image

You can build a customized image based on Fluentd's `onbuild` image.
Customized image can include plugins and `fluent.conf` file.

### 1. Create a working directory

We will use this directory to build a Docker image.
Type following commands on a terminal to prepare a minimal project first:

```bash
# Create project directory.
mkdir custom-fluentd
cd custom-fluentd

# Download default fluent.conf. This file will be copied to the new image.
# VERSION is v1.3 or v0.12 like fluentd version and OS is alpine or debian.
# Full example is https://raw.githubusercontent.com/fluent/fluentd-docker-image/master/v0.12/debian-onbuild/fluent.conf
curl https://raw.githubusercontent.com/fluent/fluentd-docker-image/master/VERSION/OS-onbuild/fluent.conf > fluent.conf

# Create plugins directory. plugins scripts put here will be copied to the new image.
mkdir plugins

curl https://raw.githubusercontent.com/fluent/fluentd-docker-image/master/Dockerfile.sample > Dockerfile
```

### 2. Customize `fluent.conf`

Documentation of `fluent.conf` is available at [docs.fluentd.org][3].

### 3. Customize Dockerfile to install plugins (optional)

You can install [Fluentd plugins][4] using Dockerfile.
Sample Dockerfile installs `fluent-plugin-elasticsearch`.
To add plugins, edit `Dockerfile` as following:

### 3.1 For current images

#### Alpine version

```Dockerfile
FROM fluent/fluentd:v1.3-onbuild-1

# Use root account to use apk
USER root

# below RUN includes plugin as examples elasticsearch is not required
# you may customize including plugins as you wish
RUN apk add --no-cache --update --virtual .build-deps \
        sudo build-base ruby-dev \
 && sudo gem install \
        fluent-plugin-elasticsearch \
 && sudo gem sources --clear-all \
 && apk del .build-deps \
 && rm -rf /home/fluent/.gem/ruby/2.5.0/cache/*.gem

USER fluent
```

#### Debian version

```Dockerfile
FROM fluent/fluentd:v1.3-debian-onbuild-1

# Use root account to use apt
USER root

# below RUN includes plugin as examples elasticsearch is not required
# you may customize including plugins as you wish
RUN buildDeps="sudo make gcc g++ libc-dev ruby-dev" \
 && apt-get update \
 && apt-get install -y --no-install-recommends $buildDeps \
 && sudo gem install \
        fluent-plugin-elasticsearch \
 && sudo gem sources --clear-all \
 && SUDO_FORCE_REMOVE=yes \
    apt-get purge -y --auto-remove \
                  -o APT::AutoRemove::RecommendsImportant=false \
                  $buildDeps \
 && rm -rf /var/lib/apt/lists/* \
           /home/fluent/.gem/ruby/2.3.0/cache/*.gem

USER fluent
```

#### Note

These example run `apk add`/`apt-get install` to be able to install
Fluentd plugins which require native extensions (they are removed immediately
after plugin installation).
If you're sure that plugins don't include native extensions, you can omit it
to make image build faster.

### 3.2 For older images

#### Alpine version

```Dockerfile
FROM fluent/fluentd:v1.3.2-onbuild

# below RUN includes plugin as examples elasticsearch is not required
# you may customize including plugins as you wish

RUN apk add --no-cache --update --virtual .build-deps \
        sudo build-base ruby-dev \
 && sudo gem install \
        fluent-plugin-elasticsearch \
 && sudo gem sources --clear-all \
 && apk del .build-deps \
 && rm -rf /home/fluent/.gem/ruby/2.5.0/cache/*.gem
```

#### Debian version

```Dockerfile
FROM fluent/fluentd:v1.3.2-debian-onbuild

# below RUN includes plugin as examples elasticsearch is not required
# you may customize including plugins as you wish

RUN buildDeps="sudo make gcc g++ libc-dev ruby-dev" \
 && apt-get update \
 && apt-get install -y --no-install-recommends $buildDeps \
 && sudo gem install \
        fluent-plugin-elasticsearch \
 && sudo gem sources --clear-all \
 && SUDO_FORCE_REMOVE=yes \
    apt-get purge -y --auto-remove \
                  -o APT::AutoRemove::RecommendsImportant=false \
                  $buildDeps \
 && rm -rf /var/lib/apt/lists/* \
           /home/fluent/.gem/ruby/2.3.0/cache/*.gem
```

### 4. Build image

Use `docker build` command to build the image.
This example names the image as `custom-fluentd:latest`:

```bash
docker build -t custom-fluentd:latest ./
```

### 5. Test it

Once the image is built, it's ready to run.
Following commands run Fluentd sharing `./log` directory with the host machine:

```bash
mkdir -p log
docker run -it --rm --name custom-docker-fluent-logger -v $(pwd)/log:/fluentd/log custom-fluentd:latest
```

Open another terminal and type following command to inspect IP address.
Fluentd is running on this IP address:

```bash
docker inspect -f '{{.NetworkSettings.IPAddress}}' custom-docker-fluent-logger
```

Let's try to use another docker container to send its logs to Fluentd.

```bash
docker run --log-driver=fluentd --log-opt tag="docker.{{.ID}}" --log-opt fluentd-address=FLUENTD.ADD.RE.SS:24224 python:alpine echo Hello
# and force flush buffered logs
docker kill -s USR1 custom-docker-fluent-logger
```
(replace `FLUENTD.ADD.RE.SS` with actual IP address you inspected at
the previous step)

You will see some logs sent to Fluentd.

### References

[Docker Logging | fluentd.org][5]

[Fluentd logging driver - Docker Docs][6]

## Issues

We can't notice comments in the DockerHub so don't use them for reporting issue or asking question.

If you have any problems with or questions about this image, please contact us
through a [GitHub issue](https://github.com/fluent/fluentd-docker-image/issues).

[1]: http://alpinelinux.org
[2]: https://hub.docker.com/_/alpine
[3]: https://docs.fluentd.org
[4]: https://www.fluentd.org/plugins
[5]: https://www.fluentd.org/guides/recipes/docker-logging
[6]: https://docs.docker.com/engine/reference/logging/fluentd
[7]: https://hub.docker.com/_/debian
[101]: https://github.com/fluent/fluentd-docker-image/blob/master/v0.12/alpine/Dockerfile
[102]: https://github.com/fluent/fluentd-docker-image/blob/master/v0.12/alpine-onbuild/Dockerfile
[105]: https://github.com/fluent/fluentd-docker-image/blob/master/v0.12/debian/Dockerfile
[106]: https://github.com/fluent/fluentd-docker-image/blob/master/v0.12/debian-onbuild/Dockerfile
[113]: https://github.com/fluent/fluentd-docker-image/blob/master/v1.3/alpine/Dockerfile
[114]: https://github.com/fluent/fluentd-docker-image/blob/master/v1.3/alpine-onbuild/Dockerfile
[115]: https://github.com/fluent/fluentd-docker-image/blob/master/v1.3/debian/Dockerfile
[116]: https://github.com/fluent/fluentd-docker-image/blob/master/v1.3/debian-onbuild/Dockerfile
