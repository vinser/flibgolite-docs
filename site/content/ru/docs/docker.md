---
author: Serguei Vine
title: Использование Docker
description: Запуск FLibGoLite в контейнере Docker.
lastmod: 2024-06-12
slug: использование-docker
layout: docs
---

Как вариант, вы можете запустить __FLibGoLite__ в `docker-контейнере`

#### Создайте докер-контейнер

```
docker build -t flibgolite .
```

#### Загрузите готовый докер-контейнер из dockerhub

TODO

#### Запустите докер-контейнер

```
docker run -d --name=flibgolite -v /srv/flibgolite:/var/flibgolite -p 8085:8085 flibgolite:latest
```

#### Используйте докер-контейнер

Поместите свои книги в `/srv/flibgolite/books/new` для импорта новых книг в свою коллекцию.

Запустите `docker logs flibgolite`, чтобы прочитать журналы.

База данных SQLite находится по пути `/srv/flibgolite/dbdata/flibgolite.db`. Делайте резервные копии регулярно.