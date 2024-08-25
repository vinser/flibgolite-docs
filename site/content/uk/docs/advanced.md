---
author: Serguei Vine
title: Розширений посібник
description: Розширене та точне налаштування
lastmod: 2024-08-25
slug: розширений-посібник
layout: docs
---

## Розширене використання

Щоб переглянути параметри запуску, в командному рядку наберіть `./flibgolite -help`
```console
Використання: flibgolite [ПАРАМЕТР] [каталог даних]

Без ПАРАМЕТРІВ програма працюватиме в режимі консолі (Ctrl+C для виходу)
Застереження: одночасно можна використовувати лише один ПАРАМЕТР

ПАРАМЕТР повинен бути одним з:
  -service [дія] - управління системним сервісом FLibGoLite
    де дія одна з:
      install (установити),
      start (запустити),
      stop (зупинити),
      restart (перезапустити),
      uninstall (видалити),
      status (статус)
  -reindex - очистити каталог, а потім сканувати папку з книгами, щоб наново додати книги до бази даних
  -config - створити файл конфігурації за промовчанням у папці ./config для налаштування
  -help - відобразити цю довідку
  -version - вивести інформацію про версію

каталог даних - необов'язковий (типовим є поточний каталог)

```

Приклади:
{{< nav id="tabs-1" type="tabs" fade="true" >}}
  {{< nav-item header="OS:" disabled=true />}}

  {{< nav-item header="Windows" show="true" >}}
    Відкрийте Powershell від імені адміністратора та виконайте команди
    {{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
      flibgolite                     ##Запустіть FLibGoLite у консольному режимі

      flibgolite -service install    ##Встановити FLibGoLite як системну службу
      flibgolite -service start      ##Запустити службу FLibGoLite	
    {{</* /command */>}}
  {{< /nav-item >}}

  {{< nav-item header="macOS" >}}
    Відкрийте термінал і виконайте команди, використовуючи `sudo`
    {{</* command user="user" host="localhost" */>}}
      ./flibgolite                          ##Запустіть FLibGoLite у консольному режимі

      sudo ./flibgolite -service install    ##Встановити FLibGoLite як системну службу
      sudo ./flibgolite -service start      ##Запустити службу FLibGoLite	
    {{</* /command */>}}
  {{< /nav-item >}}

  {{< nav-item header="Linux" >}}
    Відкрийте термінал і виконайте команди, використовуючи `sudo`
    {{</* command user="user" host="localhost" */>}}
      ./flibgolite                          ##Запустіть FLibGoLite у консольному режимі

      sudo ./flibgolite -service install    ##Встановити FLibGoLite як системну службу
      sudo ./flibgolite -service start      ##Запустити службу FLibGoLite	
    {{</* /command */>}}
  {{< /nav-item >}}

{{< /nav >}}

## Тонка настройка

### _1. Головний файл налаштування_

Зрозумілий файл конфігурації `config.yml` із деревом папок створюється під час першого запуску програми. Цей файл зберігається в підпапці `config` розташування програми. Для розширеного налаштування його можна редагувати.  
За замовчуванням такий:
{{< file path="./assets/files/config.yml" id="flibgolite-config" full="false" show="false" >}}

### _2. Налаштування розташування папок_

Щоб змінити розташування папки, просто відредагуйте відповідний рядок в `config.yml`
Наприклад, якщо потрібно створити окрему папку для нових придбаних книг, розкоментуйте рядок
```yml
NEW: "books/new"
``` 
та замініть `books/new` на відповідний шлях до папки.

### _3. Налаштування OPDS_

Ви можете змінити HTTP-порт OPDS за замовчуванням 8085 на свій
```yml
# OPDS-server port so opds can be found at http://<server name or IP-address>:8085/opds
PORT: 8085
```
Тут можна встановити бажане ім'я сервера OPDS
```yml
# OPDS-server title that is displayed in a book reader
TITLE: "FLib Go Go Go!!!"
```
Ви можете змінити кількість книг, які ваша читалка завантажуватиме за раз під час прогортання сторінки (оновлення екрана)
```yml
# OPDS feeds entries page size
PAGE_SIZE: 30
```
_Не встановлюйте це значення більше, ніж за замовчуванням. З меншими значеннями він оновлюється швидше._  
{.small}

Ви можете вимкнути миттєве перетворення FB2 у формат EPUB, просто розкоментуйте рядок, щоб встановити для NO_CONVERSION значення `true`.
```yml
# Do not convert FB2 to EPUB format
NO_CONVERSION: true
```

### _4. Поради щодо локалізації_

Для додаткової локалізації є кілька простих налаштувань.

4.1. За замовчуванням обробка нових книг обмежена англійськими, українськими та руськими книгами. Ви можете розширити цей перелік, додавши потрібні {{< link ietf_language_tags >}}теги IETF{{< /link >}}, такі як `de`, `fr`, `it` тощо.
```yml
# Accept only these languages publications. Add others if needed please.
ACCEPTED: "en, ru, uk"
```  

4.2. За замовчуванням читалка буде відображати меню і коментарі англійською мовою `en`. Якщо хочете українською, можете змінити цей параметр на `uk`.
```yml
# Default english locale for opds feeds (bookreaders opds menu tree) can be changed to:
# "uk" for Ukrainian, 
# "ru" for Russian 
DEFAULT: "en"
```

### _5. База даних списку книг_

5.1. Список книг зберігається у файлі `books.db` бази даних SQLite, розташованому в папці `dbdata`. 
```yml
DSN: "dbdata/books.db"
```
> [!CAUTION]
> Файл бази даних створюється при першому запуску програми і __не підтримує редагування__

5.2. У разі втрати або пошкодження файл бази даних може бути відновлено командою
{{< nav id="tabs-2" type="tabs" fade="true" >}}
  {{< nav-item header="OS:" disabled=true />}}

  {{< nav-item header="Windows" show="true" >}}
    Відкрийте Powershell від імені адміністратора та виконайте команду
    {{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
      flibgolite -reindex
    {{</* /command */>}}
  {{< /nav-item >}}

  {{< nav-item header="macOS" >}}
    Відкрийте термінал та виконайте команду, використовуючи `sudo`
    {{</* command user="user" host="localhost" */>}}
      sudo ./flibgolite -reindex
    {{</* /command */>}}
  {{< /nav-item >}}

  {{< nav-item header="Linux" >}}
    Відкрийте термінал та виконайте команду, використовуючи `sudo`
    {{</* command user="user" host="localhost" */>}}
      sudo ./flibgolite -reindex
    {{</* /command */>}}
  {{< /nav-item >}}
{{< /nav >}}

Це може тривати деякий час.

5.3. Процес реєстрації нових надходжень зображено на діаграмі
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
У більшості випадків процес реєстрації нових придбань не вимагає налаштування, оскільки його швидкість визначається в основному продуктивністю системи введення-виведення (контролерами, типами дисків тощо). За необхідності змінити роботу процесу можна такими налаштуваннями:
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

### _6. Ведення журналу_

При роботі сервіс записує журнали `opds.log` та `scan.log`, розташовані в папці `logs`.

```yml
OPDS: "logs/opds.log"
SCAN: "logs/scan.log"
```
`opds.log` містить записи про запити читалок.
`scan.log` містить записи про нові книги та індексацію архіву.
Щоб перенаправити виведення журналу на консоль (stdout), просто закоментуйте відповідний рядок OPDS або SCAN.

Немає необхідності видаляти журнали, щоб звільнити місце на диску, оскільки журнали змінюються (перезаписуються) через 7 днів.

Ви можете налаштувати рівень ведення журналу (подробиця) на один із можливих значень: `D` - налагодження, `I` - інформація, `W` - попередження (за замовчуванням), `E` - помилки
```yml
LEVEL: "W"
```

-------------------------------
___*Пропозиції, повідомлення про помилки та коментарі вітаються {{< link repository_issues >}}тут{{< /link >}}*___
   

