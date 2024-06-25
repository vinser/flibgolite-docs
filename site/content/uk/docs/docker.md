---
author: Serguei Vine
title: Використання Docker
description: Запуск FLibGoLite у контейнері Docker
date: 2024-06-12
slug: використання-docker
layout: docs
---

### Створіть докер-контейнер

```
docker build -t flibgolite .
```

### Завантажте готовий до використання контейнер з dockerhub

TODO

### Запустіть докер-контейнер 

```
docker run -d --name=flibgolite -v /srv/flibgolite:/var/flibgolite -p 8085:8085 flibgolite:latest
```

### Використовуйте докер-контейнер

Помістіть свої книги в `/srv/flibgolite/books/new`, щоб імпортувати нові книги у свою колекцію.

Запустіть `docker logs flibgolite`, щоб прочитати журнали.

База даних SQLite знаходиться на шляху `/srv/flibgolite/dbdata/flibgolite.db`. Регулярно створюйте резервні копії.
