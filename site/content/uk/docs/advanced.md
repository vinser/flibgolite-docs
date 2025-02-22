---
author: Serguei Vine
title: Розширений посібник
description: Розширене та точне налаштування
lastmod: 2025-02-22
slug: розширений-посібник
layout: docs
---

## Параметри запуску

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

  {{< nav-item header="macOS, Linux, FreeBSD" >}}
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

Зрозумілий файл конфігурації `config.yml` створюється разом із деревом папок при першому запуску програми в підпапці `config` папки, в якій лежить виконуваний файл програми `flibgolite`. Для тонкої будови `config.yml` можна редагувати.  
За замовчуванням такий:
{{< file path="./assets/files/config.yml" id="flibgolite-config" full="false" show="false" >}}

### _2. Налаштування розташування папки з книгами_

За замовчуванням FLibGoLite додає до OPDS-каталогу книги з папки `books/stock` і нікуди їх не переміщає. Щоб змінити розташування папки, просто відредагуйте відповідний рядок в `config.yml`  
```yml
  STOCK: "books/stock" # Book stock
``` 
та замініть `books/stock` на відповідний шлях до папки.
>[!CAUTION]
>У шляхах замість зворотного слеша `\` використовуйте прямий слеш `/`.

Також це можна зробити за допомогою створення посилання на іншу папку на диску, де ваші книги лежать.  
Наприклад:
{{< nav id="tabs-2" type="tabs" fade="true" >}}

{{< nav-item header="OS:" disabled=true />}}

{{< nav-item header="Windows" show="true" >}}
Відкрийте Powershell та виконайте команду створення з'єднання каталогів
{{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
mklink /J.\books D:\path\to\books
{{</* /command */>}}
{{< /nav-item >}}

{{< nav-item header="macOS, Linux, FreeBSD" >}}
Відкрийте термінал та виконайте команду створення символьного посилання
{{</* command user="user" host="localhost" */>}}
ln -s /path/to/books ./books
{{</* /command */>}}
{{< /nav-item >}}

{{< /nav >}}

### _3. Налаштування OPDS_

3.1. Ви можете змінити HTTP-порт OPDS за замовчуванням 8085 на свій
```yml
# OPDS-server port so opds can be found at http://<server name or IP-address>:8085/opds
PORT: 8085
```
3.2. Тут можна встановити бажане ім'я сервера OPDS
```yml
# OPDS-server title that is displayed in a book reader
TITLE: "FLib Go Go Go!!!"
```
3.3. Ви можете змінити кількість книг, які ваша читалка завантажуватиме за раз під час прогортання сторінки (оновлення екрана)
```yml
# OPDS feeds entries page size
PAGE_SIZE: 20
```
>[!WARNING]
>Не встановлюйте це значення значно більше значення за замовчуванням. Це може зробити оновлення сторінки або прокручування екрана некомфортним.  

3.4. Ви можете вимкнути миттєве перетворення FB2 у формат EPUB, просто розкоментуйте рядок, щоб встановити для NO_CONVERSION значення `true`.
```yml
# Do not convert FB2 to EPUB format
NO_CONVERSION: true
```

### _4. Поради щодо локалізації_

Для додаткової локалізації є кілька простих налаштувань.

4.1. За замовчуванням обробка нових книг обмежена англійськими, українськими та руськими книгами. Ви можете розширити цей перелік, додавши потрібні {{< link ietf_language_tags >}}теги IETF{{< /link >}}, такі як `de`, `fr`, `it` тощо.
```yml
# Accept only these languages publications. Add others if needed please.  Set "any" to accept any language 
ACCEPTED: "en, ru, uk"
```  
>[!NOTE]  
>Установіть значення `any`, щоб приймати книги будь-якою мовою
```yml
ACCEPTED: "any"
```   

4.2. За замовчуванням читалка буде відображати меню і коментарі англійською мовою `en`. Якщо хочете українською, можете змінити цей параметр на `uk`.
```yml
# Default english locale for opds feeds (bookreaders opds menu tree) can be changed to:
# "uk" for Ukrainian, 
# "ru" for Russian 
DEFAULT: "en"
```

Якщо у першому запиті до каталогу OPDS програма для читання книг надсилає заголовок `Accept-Language` із специфікацією мови, запитувана мова буде встановлена ​​за замовчуванням.

>[!NOTE]  
>Щоб залишити лише одну мову, ви можете видалити всі файли локалі в папці `config/locales`, крім вибраної мови за замовчуванням. У цьому випадку рідер не відобразить меню вибору мови. Дивіться Розділ 4.3 про файли локалі.

4.3. Якщо ваша рідна мова є іншою, ніж три згадані вище, для вашої зручності ви можете створити власний файл локалі та помістити його в папку `config/locales`. 

```yml
# Locales folder. You can add your own locale file there like en.yml, ru.yml, uk.yml
DIR: "config/locales"
```

Наприклад, для німецької мови скопіюйте `en.yml` в `de.yml` і перекладіть фрази німецькою мовою праворуч від двокрапки-роздільника. Залиште символи формату `%d` недоторканими. Щось на зразок цього: 

```yml
Found authors - %d: Autoren gefunden - %d
```

Не забудьте замінити рядок алфавіту `ABC` на німецький. Це забезпечить правильне відображення та сортування німецьких імен і назв.  

4.4. Адаптацію мови вибору дерева жанрів можна здійснити шляхом редагування файлу `genres.xml` у папці `config`

```yml
  TREE_FILE: "config/genres.xml"
```

Це можна зробити, додавши рядки для певної мови у файл `genres.xml`.

```xml
<genre-descr lang="en" title="Alternative history"/>
<genre-descr lang="ru" title="Альтернативная история"/>
<genre-descr lang="uk" title="Альтернативна історія"/>
<genre-descr lang="de" title="Alternative Geschichte"/>
```

### _5. База даних списку книг_

5.1. Список книг зберігається у файлах бази даних SQLite, розміщених у папці `dbdata`. 
```yml
DSN: "dbdata/books.db"
```
> [!CAUTION]
> Файли бази даних створюються при першому запуску програми та __не призначені для ручного редагування__

5.2. У разі втрати або пошкодження файли бази даних можуть бути відновлені командою
{{< nav id="tabs-2" type="tabs" fade="true" >}}

  {{< nav-item header="OS:" disabled=true />}}

  {{< nav-item header="Windows" show="true" >}}
    Відкрийте Powershell від імені адміністратора та виконайте команду
    {{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
      flibgolite -reindex
    {{</* /command */>}}
  {{< /nav-item >}}

  {{< nav-item header="macOS, Linux, FreeBSD" >}}
    Відкрийте термінал та виконайте команду, використовуючи `sudo`
    {{</* command user="user" host="localhost" */>}}
      sudo ./flibgolite -reindex
    {{</* /command */>}}
  {{< /nav-item >}}

{{< /nav >}}

Це може тривати деякий час.
>[!NOTE]  
>Те саме можна виконати вручну: зупинити сервіс, видалити файли бази даних і запустити сервіс заново.  

5.3. Процес реєстрації нових надходжень зображено на діаграмі
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
У більшості випадків процес реєстрації нових придбань не вимагає налаштування, оскільки його швидкість визначається в основному продуктивністю системи введення-виведення (контролерами, типами дисків тощо). За необхідності змінити роботу процесу можна такими налаштуваннями:
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
5.4. При додаванні нових книг у каталог можна налаштувати відбраковування дублікатів:
- `N` - не перевіряти,
- `F` - перевіряти тільки за збігом хеша CRC32 (за умовчанням),
- `S` - перевіряти за збігом CRC32 або за назвою книги та за коротким описом. 
```yml
  # Level of checking new books for duplicates: N - no check, F - fast check (default) by CRC32, S - slow check by CRC32 or title and plot comparison
  DEDUPLICATE_LEVEL: "F"
```

### _6. Ведення журналу_

При роботі сервіс записує журнали `opds.log` та `scan.log`, розташовані в папці `logs`.

```yml
  # Logs are here
  OPDS: "logs/opds.log"
  SCAN: "logs/scan.log"
```
`opds.log` містить записи про запити читалок.
`scan.log` містить записи про нові книги та індексацію архіву.

Немає необхідності видаляти журнали, щоб звільнити місце на диску, оскільки журнали змінюються (перезаписуються) через 7 днів.

Можна задати один із наступних рівнів деталізації журналу:
- `D` - налагодження,
- `I` - інформація,
- `W` - попередження (за замовчуванням),
- `E` - помилки.
```yml
  # Logging levels: D - debug, I - info, W - warnings (default), E - errors
  LEVEL: "W" 
```

-------------------------------
___*Пропозиції, повідомлення про помилки та коментарі вітаються {{< link repository_issues >}}тут{{< /link >}}*___
   

