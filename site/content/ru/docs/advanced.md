---
author: Serguei Vine
title: Расширенное руководство
description: Расширенная и тонкая настройка
lastmod: 2025-01-06
slug: расширенное-руководство
layout: docs
---

## Параметры запуска

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

{{< nav-item header="macOS, Linux, FreeBSD" >}}
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

Понятный файл конфигурации `config.yml` создается вместе с деревом папок при первом запуске программы в подпапке `config` папки, в которой лежит исполняемый файл программы `flibgolite`. Для тонкой настройки `config.yml` можно редактировать.  
Содержимое по умолчанию следующее:
{{< file path="./assets/files/config.yml" id="flibgolite-config" full="false" show="false" >}}

### _2. Настройка расположения папки с книгами_

По умолчанию FLibGoLite добавляет в OPDS-каталог книги из папки `books/stock` и никуда их не перемещает. Чтобы изменить местоположение папки, просто отредактируйте соответствующую строку в `config.yml`
```yml
  STOCK: "books/stock" # Book stock
``` 
и замените `books/stock` на нужный вам путь к папке.  

Также это можно сделать при помощи создания ссылки на другую папку на диске, где лежат ваши книги.  
Например:
{{< nav id="tabs-2" type="tabs" fade="true" >}}

{{< nav-item header="OS:" disabled=true />}}

{{< nav-item header="Windows" show="true" >}}
Откройте Powershell и выполните команду создания соединения каталогов
{{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
mklink /J .\books D:\path\to\books
{{</* /command */>}}
{{< /nav-item >}}

{{< nav-item header="macOS, Linux, FreeBSD" >}}
Откройте терминал и выполните команду создания символьной ссылки
{{</* command user="user" host="localhost" */>}}
ln -s /path/to/books ./books
{{</* /command */>}}
{{< /nav-item >}}

{{< /nav >}}

### _3. Настройка OPDS_

3.1. Вы можете изменить HTTP-порт OPDS по умолчанию 8085 на свой
```yml
# OPDS-server port so opds can be found at http://<server name or IP-address>:8085/opds
PORT: 8085
```
3.2. Здесь вы можете установить желаемое имя сервера OPDS, отображаемое в читалке
```yml
# OPDS-server title that is displayed in a book reader
TITLE: "FLib Go Go Go!!!"
```
3.3. Вы можете изменить количество книг, которые ваша читалка будет загружать за раз при пролистывании страницы или обновлении экрана
```yml
# OPDS feeds entries page size
PAGE_SIZE: 20
```
>[!WARNING]
>Не устанавливайте это значение существенно больше значения по умолчанию. Это может сделать обновление страницы или пролистывание экрана некомфортным.  

3.4. Вы можете отключить преобразование формата FB2 в EPUB при загрузке книг на читалку, просто раскомментируйте строку, чтобы установить NO_CONVERSION в значение `true`
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

5.1. Список книг хранится в файлах базы данных SQLite, расположенных в папке `dbdata`.
```yml
DSN: "dbdata/books.db"
```
> [!CAUTION]
> Файлы базы данных создаются при первом запуске программы и __не предназначены для ручного редактирования__

5.2. В случае утраты или повреждения файлы базы данных могут быть восстановлены командой
{{< nav id="tabs-2" type="tabs" fade="true" >}}

{{< nav-item header="OS:" disabled=true />}}

{{< nav-item header="Windows" show="true" >}}
  Откройте Powershell от имени администратора и выполните команду
  {{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
    flibgolite -reindex  
  {{</* /command */>}}
{{< /nav-item >}}

{{< nav-item header="macOS, Linux, FreeBSD" >}}
  Откройте терминал и выполните команду, используя `sudo`
  {{</* command user="user" host="localhost" */>}}
    sudo ./flibgolite -reindex
  {{</* /command */>}}
{{< /nav-item >}}

{{< /nav >}}

Это может занять некоторое время.
>[!NOTE]  
>То же самое можно выполнить вручную: остановить сервис, удалить файлы базы данных и запустить сервис заново.  

5.3. Процесс регистрации новых поступлений изображен на диаграмме
```goat
 .----------------------+
| STOCK folder scanning |
+------+---------------'
       |
       v                                                                                              
+------------------------+
| Scan every 300 seconds |
+------+-----------------+
       |                                                                                              
       v                                                                                           .---------. 
       |   +----------------------+       .-------------------+      +---------------------+      |           |   
       +-->| Parallel EPUB parser +--+-->| Parsed books queue +----->| Bulked Tx worker    +----->|'---------'|
       |   +----------------------+  |   |--------------------|      |---------------------|      |           |
       |                             |   | max 20000 books    |      | max 20000 at a time |      | SQLite DB |
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
                                         | max 20000 files     | 
                                         +--------------------'
       
```
В большинстве случаев процесс регистрации поступлений не требует настройки, поскольку его скорость определяется в основном производительностью системы ввода-вывода (контроллерами, типами дисков и т.п.). При необходимости изменить работу процесса можно следующими настройками: 
```yml
  # Delay before start each new acquisitions folder processing
  POLL_DELAY: 300 
  # Maximum parallel new acquisitions processing routines
  MAX_SCAN_THREADS: 10
  # Book queue size
  BOOK_QUEUE_SIZE: 20000
  # File queue size
  FILE_QUEUE_SIZE: 20000
  # Maximum number of books in one transaction
  MAX_BOOKS_IN_TX: 20000
```
5.4. При добавлении новых книг в каталог можно настроить отбраковку дубликатов:  
- `N` - не проверять,   
- `F` - проверять только по совпадению хеша CRC32 (по умолчанию),   
- `S` - проверять по совпадению CRC32 или по названию книги и краткому описанию.  
```yml
  # Level of checking new books for duplicates: N - no check, F - fast check (default) by CRC32, S - slow check by CRC32 or title and plot comparison
  DEDUPLICATE_LEVEL: "F"
```

### _6. Ведение журнала_

При работе сервис записывает журналы `opds.log` и `scan.log`, расположенные в папке `logs`.

```yml
  # Logs are here
  OPDS: "logs/opds.log"
  SCAN: "logs/scan.log"
```
`opds.log` содержит записи о запросах читалок.
`scan.log` содержит записи о новых книгах и индексации архива.

Нет необходимости удалять журналы, чтобы освободить место на диске, поскольку журналы меняются (перезаписываются) через 7 дней.

Можно задать один из следующих уровеней детализации журнала:  
- `D` - отладка,
- `I` - информация,
- `W` - предупреждения (по умолчанию),
- `E` - ошибки.
```yml
  # Logging levels: D - debug, I - info, W - warnings (default), E - errors
  LEVEL: "W" 
```

-------------------------------
___*Предложения, отчеты об ошибках и комментарии приветствуются {{< link repository_issues >}}здесь{{< /link >}}*___