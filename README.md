# Docker Image for Bruno

This project contains a Dockerfile to build an image for
[Bruno](https://github.com/usebruno/bruno), a tool for testing and
exploring web APIs.

## Build

You can build the image from the remote Git repo, without having to check it out even:

```shell
docker build -t bruno-image github.com/davidkarlsen/bruno-image
```

## Download

There are prebuilt images available in the GitHub Container Registry, so
you can simply use that:
```shell
# fetch help output
docker run --rm ghcr.io/davidkarlsen/bruno-image help
# run bruno on the collection in the current directory
docker run --rm --volume .:/bruno ghcr.io/davidkarlsen/bruno-image
```

## Usage

### Plain Docker

```shell
# fetch help output
docker run --rm bruno-image help
# run bruno on the collection in the current directory
docker run --rm --volume .:/bruno bruno-image
```

### Docker Compose

You can integrate this as a service to run the tests (typically against
other services in the setup) in the isolation of a container.

```yaml
version: '3.6'

services:
  bruno:
    build: github.com/davidkarlsen/bruno-image
    volumes:
      # mount collection into the container
      - type: bind
        source: .
        target: /bruno
    profiles:
      # set a profile to prevent this from being started by default
      - bruno
```

```shell
# fetch help output
docker compose run bruno help
# run bruno on the collection in the mounted directory
docker compose run bruno
```
