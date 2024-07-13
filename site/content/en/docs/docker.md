---
author: Serguei Vine
title: Using Docker
description: Run FLibGoLite in Docker container
date: 2024-06-12
slug: using-docker
layout: docs
---
As an option you may run __FLibGoLite__ in `docker container`

#### Build docker container

```
docker build -t flibgolite .
```

#### Download ready-to-use docker container from dockerhub

TODO

#### Run docker container

```
docker run -d --name=flibgolite -v /srv/flibgolite:/var/flibgolite -p 8085:8085 flibgolite:latest
```

#### Use docker container

Put your books into `/srv/flibgolite/books/new` to import new books into your collection.

Run `docker logs flibgolite` to read the logs.

The SQLite database is on the path `/srv/flibgolite/dbdata/flibgolite.db`. Back it up on a regular basis.
