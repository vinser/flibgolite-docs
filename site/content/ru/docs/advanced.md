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
Использование: flibgolite [ПАРАМЕТР] [каталог данных]

Без ПАРАМЕТРОВ программа будет работать в консольном режиме (Ctrl+C для выхода)
Внимание: одновременно можно использовать только один ПАРАМЕТР.

ПАРАМЕТР должен быть одним из:
 -service [действие] - управление системным сервисом FLibGoLite
    где действие одно из: 
      install (установить), 
      start (запустить),
      stop (остановить), 
      restart (перезапустить), 
      uninstall (удалить), 
      status (статус)
 -reindex - очистить каталог, а затем сканировать папку с книгами, чтобы заново добавить книги в базу данных
 -config - создать файл конфигурации по умолчанию в папке ./config для настройки
 -help - отобразить эту справку
 -version - вывести информацию о версии

каталог данных - не является обязательным (по умолчанию это текущий каталог)
 ```

Примеры:
{{< nav id="tabs-1" type="tabs" fade="true" >}}
{{< nav-item header="OS:" disabled=true />}}

{{< nav-item header="Windows" show="true" >}}
Откройте Powershell от имени администратора и выполняйте команды
{{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
flibgolite                     ##Запустить FLibGoLite в консольном режиме

flibgolite -service install    ##Установить FLibGoLite как системную службу
flibgolite -service start      ##Запустить службу FLibGoLite	
{{</* /command */>}}

{{< /nav-item >}}
{{< nav-item header="macOS" >}}
Откройте терминал и выполняйте команды, используя `sudo`
{{</* command user="user" host="localhost" */>}}
./flibgolite                          ##Запустить FLibGoLite в консольном режиме

sudo ./flibgolite -service install    ##Установить FLibGoLite как системную службу
sudo ./flibgolite -service start      ##Запустить службу FLibGoLite	
{{</* /command */>}}

{{< /nav-item >}}
{{< nav-item header="Linux" >}}
Откройте терминал и выполняйте команды, используя `sudo`
{{</* command user="user" host="localhost" */>}}
./flibgolite                          ##Запустить FLibGoLite в консольном режиме

sudo ./flibgolite -service install    ##Установить FLibGoLite как системную службу
sudo ./flibgolite -service start      ##Запустить службу FLibGoLite	
{{</* /command */>}}

{{< /nav-item >}}
{{< /nav >}}

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
_Не устанавливайте это значение больше значения по умолчанию. При более низких значениях обновление происходит быстрее._  
{.small}

Вы можете отключить преобразование формата FB2 в EPUB, просто раскомментируйте строку, чтобы установить NO_CONVERSION в значение `true`
```yml
# Do not convert FB2 to EPUB format
NO_CONVERSION: true
```

### _4. Советы по локализации_

Для дополнительной локализации есть несколько простых настроек.

4.1. По умолчанию обработка новых книг ограничена английскими, русскими и украинскими книгами. Вы можете расширить этот перечень добавив нужные {{< link ietf_language_tags >}}теги IETF{{< /link >}}, такие как `de`, `fr`, `it` и т. п.
```yml
# Accept only these languages publications. Add others if needed please.
ACCEPTED: "en, ru, uk"
```  

4.2. По умолчанию читалка будет отображать меню и комментарии на английском языке `en`, если вы хотите на русском, вы можете изменить эту настройку на `ru`.
```yml
# Default english locale for opds feeds (bookreaders opds menu tree) can be changed to:
# "uk" for Ukrainian, 
# "ru" for Russian 
DEFAULT: "en"
```

### _5. База данных перечня книг_

5.1. Перечень книг хранится в файле `books.db` базы данных SQLite, расположенном в папке `dbdata`.
```yml
DSN: "dbdata/books.db"
```
{{< alert type="danger" >}}
  Файл базы данных создается при первом запуске программы и __не предназначен для ручного редактирования__
{{< /alert >}}

5.2. В случае утраты или повреждения файл базы данных может быть восстановлен командой
{{< nav id="tabs-2" type="tabs" fade="true" >}}

{{< nav-item header="OS:" disabled=true />}}

{{< nav-item header="Windows" show="true" >}}
  Откройте Powershell от имени администратора и выполните команду
  {{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
    flibgolite -reindex  
  {{</* /command */>}}
{{< /nav-item >}}

{{< nav-item header="macOS" >}}
  Откройте терминал и выполните команду, используя `sudo`
  {{</* command user="user" host="localhost" */>}}
    sudo ./flibgolite -reindex
  {{</* /command */>}}
{{< /nav-item >}}

{{< nav-item header="Linux" >}}
  Откройте терминал и выполните команду, используя `sudo`
  {{</* command user="user" host="localhost" */>}}
    sudo ./flibgolite -reindex
  {{</* /command */>}}
{{< /nav-item >}}

{{< /nav >}}

Это может занять некоторое время.

5.3. Процесс регистрации новых поступлений изображен на диаграмме
```goat
 .----------------------+
| STOCK folder scanning |
+------+---------------'
       |
       v                                                                                              
+-----------------------+
| Scan every 30 seconds |
+------+----------------+
       |                                                                                              
       v                                                                                           .---------. 
       |   +----------------------+       .-------------------+      +---------------------+      |           |   
       +-->| Parallel EPUB parser +--+-->| Parsed books queue +----->| Bulked Tx worker    +----->|'---------'|
       |   +----------------------+  |   |--------------------|      |---------------------|      |           |
       |                             |   | max 1000 books     |      | max 1000 at a time  |      | SQLite DB |
       |                             |   +-------------------'       +---------------------+      |           |
       |                             |               ^                                             '---------' 
       |   +----------------------+  |               |                                                
       +-->| Parallel FB2 parser  +--+   +-----------+---------+
       |   +----------------------+      | parallel FB2 parser |
       |                                 |---------------------|
       |                                 |    10 workers       |
       |                                 +---------------------+
       |                                             ^ 
       |                                             |  
       |   +----------------------+       .----------+---------+
       +-->| Unzip FB2 zip-archive|----->| Unziped files queue |
           +----------------------+      |---------------------|
                                         | max 1000 files      | 
                                         +--------------------'
       
```
В большинстве случаев процесс регистрации поступлений не требует настройки, поскольку его скорость определяется в основном производительностью системы ввода-вывода (контроллерами, типами дисков и т.п.). При необходимости изменить работу процесса можно следующими настройками: 
```yml
 # Delay before start each new acquisitions folder processing
  POLL_DELAY: 30
  # Maximum parallel new acquisitions processing routines
  MAX_SCAN_THREADS: 10
  # Book queue size
  BOOK_QUEUE_SIZE: 1000
  # File queue size
  FILE_QUEUE_SIZE: 1000
  # Maximum number of books in one transaction
  MAX_BOOKS_IN_TX: 1000
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

-------------------------------
___*Предложения, отчеты об ошибках и комментарии приветствуются {{< link repository_issues >}}здесь{{< /link >}}*___