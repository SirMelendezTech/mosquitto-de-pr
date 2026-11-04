# Fork images

`Dockerfile.alpine` and `Dockerfile.ubuntu` build Mosquitto from this
repository's source, so the images include the fixes on `master` that are
not in an official release yet. They are derived from
`docker/2.1-alpine/Dockerfile` and `docker/2.1-ubuntu/Dockerfile`; the only
differences are that the source comes from `mosq.tar.gz` in the build
context instead of the mosquitto.org release download, and the image
labels.

The `Docker` workflow (`.github/workflows/docker.yml`) publishes them on
every push to `master` and on `v*` tags:

    ghcr.io/sirmelendeztech/mosquitto:2.1-alpine
    ghcr.io/sirmelendeztech/mosquitto:2.1-ubuntu
    ghcr.io/sirmelendeztech/mosquitto:<git describe>-alpine
    ghcr.io/sirmelendeztech/mosquitto:<git describe>-ubuntu

To build locally:

    git archive --format=tar.gz --prefix=mosquitto/ -o docker/2.1-alpine/mosq.tar.gz HEAD
    docker build -f docker/fork/Dockerfile.alpine \
        --build-arg VERSION="$(git describe --tags --always)" \
        -t mosquitto:local docker/2.1-alpine

Usage is the same as the official image; see `docker/2.1-alpine/README.md`.
