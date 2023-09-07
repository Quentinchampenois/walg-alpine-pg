# Docker environment for running Walg on a PostgreSQL 14 database

This repository contains Docker images for compiling [WAL-G](https://github.com/wal-g/wal-g) `v2.0.1` on Alpine 3.18 OS with go `v1.20`.

Also, it contains a Docker image for running PostgreSQL 14 on Alpine with WAL-G binary already installed.

## Tree

* [walg/Dockerfile](./walg/Dockerfile) - Dockerfile for building WAL-G binary 
  * Available from Docker hub : `docker pull quentinchampenois/walg:v2.0.1-alpine3.18`
* [pg_walg/Dockerfile](pg_walg/Dockerfile) - Dockerfile for building PostgreSQL 14 with WAL-G binary already installed
  * Available from Docker hub : `docker pull quentinchampenois/pg_walg:v2.0.1-14`

## How to build this image:

You can try this image by building and running it locally:

```bash
docker run --rm -it $(docker build -q .) /bin/sh
```

Now you should be inside the container and can run `wal-g` commands:

```bash
./wal-g --help
```

## Resources

* [WAL-G](https://github.com/wal-g/wal-g)
* [PostgreSQL](https://www.postgresql.org/)
* [Alpine Linux](https://alpinelinux.org/)
* [Docker](https://www.docker.com/)
* Dockerfiles inspired from the useful playground [playground-postgresql-walg](https://github.com/stephane-klein/playground-postgresql-walg/). Dockerfiles are also compatible with this playground.
Special thanks to @stephane-klein for the playground