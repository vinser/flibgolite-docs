---
author: Serguei Vine
title: Расширенное руководство
description: Расширенная и тонкая настройка
date: 2024-06-12
slug: расширенное-руководство
layout: docs
---

## Расширенное использование
Чтобы просмотреть параметры запуска, в командной строке наберите `./flibgolite -help`
```console
Usage: flibgolite [OPTION] [data directory]

With no OPTION program will run in console mode (Ctrl+C to exit)
Caution: Only one OPTION can be used at a time

OPTION should be one of:
  -service [action]     control FLibGoLite system service
          where action is one of: install, start, stop, restart, uninstall, status 
  -reindex              empty book stock index and then scan book stock directory to add books to index (database)
  -config               create default config file in ./config folder for customization and exit
  -help                 display this help and exit
  -version              output version information and exit

data directory is optional (current directory by default)
```

Примеры:

```console
./flibgolite                          Запустить FLibGoLite в консольном режиме.

sudo ./flibgolite -service install    Установить FLibGoLite как системную службу.
sudo ./flibgolite -service start      Запустить сервис FLibGoLite
```

## Тонкая настройка

### _1. Главный файл настройки_

Понятный файл конфигурации `config.yml` с деревом папок создается при первом запуске программы. Этот файл хранится в подпапке `config` расположения программы. Для расширенной настройки его можно редактировать.  
Содержимое по умолчанию следующее:
{{< file path="./assets/files/config.yml" id="flibgolite-config" full="false" show="false" >}}

### _2. Настройка расположения папок_

Чтобы изменить местоположение папки, просто отредактируйте соответствующую строку в `config.yml`
Например, если вам нужно создать отдельную папку для новых приобретенных книг, раскомментируйте строку
```yml
NEW: "books/new"
``` 
и замените `books/new` на соответствующий путь к папке.

### _3. Настройка OPDS_

Вы можете изменить HTTP-порт OPDS по умолчанию 8085 на свой
```yml
# OPDS-server port so opds can be found at http://<server name or IP-address>:8085/opds
PORT: 8085
```
Здесь вы можете установить желаемое имя сервера OPDS
```yml
# OPDS-server title that is displayed in a book reader
TITLE: "FLib Go Go Go!!!"
```
Вы можете изменить количество книг, которые ваша читалка будет загружать за раз при пролистывании страницы (обновлении экрана)
```yml
# OPDS feeds entries page size
PAGE_SIZE: 30
```
Не устанавливайте это значение больше значения по умолчанию. При более низких значениях обновление происходит быстрее.

### _4. Советы по локализации_

Для дополнительной локализации есть несколько простых настроек.

4.1. По умолчанию обработка новых книг ограничена английскими, русскими и украинскими книгами. Вы можете добавить [другие](https://en.wikipedia.org/wiki/IETF_language_tag) языки, например `"de"`, `"fr"`, `"it"` и так далее
```yml
# Accept only these languages publications. Add others if needed please.
ACCEPTED: "en, ru, uk"
```  

4.2. По умолчанию программа чтения книг будет отображать меню и комментарии на английском языке "en", если вы хотите на русском, вы можете изменить эту настройку на "ru".
```yml
# Default english locale for opds feeds (bookreaders opds menu tree) can be changed to:
# "uk" for Ukrainian, 
# "ru" for Russian 
DEFAULT: "en"
```

### _5. База данных перечня книг_

Перечень книг хранится в файле базы данных SQLite, расположенном в папке dbdata. Он создается при первом запуске программы и __не предназначен для ручного редактирования__
```yml
DSN: "dbdata/books.db"
```
В случае его потери или повреждения он может быть восстанолен командой
```console
sudo ./flibgolite -reindex
```
Ее выполнение может занять некоторое время.
Перед выполнением команды восстановления необходимо остановить сервис
```console
sudo ./flibgolite -service stop
```
и после восстановления снова запустить
```console
sudo ./flibgolite -service start
```

### _6. Ведение журнала_

При работе сервис записывает журналы `opds.log` и `scan.log`, расположенные в папке `logs`.

```yml
OPDS: "logs/opds.log"
SCAN: "logs/scan.log"
```
`opds.log` содержит записи о запросах читалок.
`scan.log` содержит записи о новых книгах и индексации архива.
Чтобы перенаправить вывод журнала на консоль (stdout), просто закомментируйте соответствующую строку OPDS или SCAN.

Нет необходимости удалять журналы, чтобы освободить место на диске, поскольку журналы меняются (перезаписываются) через 7 дней.

Вы можете настроить уровень ведения журнала (подробность) на один из возможных значений: `D` — отладка, `I` - информация, `W` - предупреждения (по умолчанию), `E` - ошибки.
```yml
LEVEL: «W»
```
### _7. Запуск в контейнере Docker_

Как вариант, вы можете запустить программу в [docker-контейнере](docs/docker.md).

### _8. Сборка из исходников_

Если у вас есть какие-либо сомнения по поводу безопасности готовых исполняемых файлов или нет подходящего для вашего оборудования и ОС, вы можете легко создать его самостоятельно.
Чтобы собрать исполняемый файл, установите [Go](https://go.dev/dl/), [Git](https://git-scm.com/downloads), клонируйте [репозиторий FLibGoLite](https://github. com/vinser/flibgolite) и запустите
```консоль
go build ./cmd/flibgolite
```
Лучше собрать его на хосте, на котором будет работать служба. Вы получите исполняемый файл, подходящий для операционной системы и оборудования хоста.  
  
Для кросс-компиляции установите GNU make и запустите его с Makefile.


-------------------------------
___*Предложения, отчеты об ошибках и комментарии приветствуются [здесь](https://github.com/vinser/flibgolite/issues)*___