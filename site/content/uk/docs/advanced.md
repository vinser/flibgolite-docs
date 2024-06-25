---
author: Serguei Vine
title: Розширений посібник
description: Розширене та точне налаштування
date: 2024-06-12
slug: розширений-посібник
layout: docs
---

## Розширене використання
Щоб переглянути параметри запуску, в командному рядку наберіть `./flibgolite -help`
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

Приклади:

```console
./flibgolite                         Запустіть FLibGoLite у консольному режимі.

sudo ./flibgolite -service install   Встановити FLibGoLite як системну службу.
sudo ./flibgolite -service start     Запустити сервіс FLibGoLite
```

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
Не встановлюйте це значення більше, ніж за замовчуванням. З меншими значеннями він оновлюється швидше.

### _4. Поради щодо локалізації_

Для додаткової локалізації є кілька простих налаштувань.

4.1. За замовчуванням обробка нових книг обмежена англійськими, українськими та руськими книгами. Ви можете додати [інші](https://en.wikipedia.org/wiki/IETF_language_tag) мови, наприклад ``de'`, `fr', `it' і так далі
```yml
# Accept only these languages publications. Add others if needed please.
ACCEPTED: "en, ru, uk"
```  

4.2. За замовчуванням читалка буде відображати меню і коментарі англійською мовою "en". Якщо хочете українською, можете змінити цей параметр На "uk".
```yml
# Default english locale for opds feeds (bookreaders opds menu tree) can be changed to:
# "uk" for Ukrainian, 
# "ru" for Russian 
DEFAULT: "en"
```

### _5. База даних списку книг_

Список книг зберігається у файлі бази даних SQLite, розташованому в папці dbdata. Він створюється при першому запуску програми і не призначений для ручного редагування.
```yml
DSN: "dbdata/books.db"
```
У разі втрати або пошкодження його може бути відновлено командою
```console
sudo ./flibgolite -reindex
```
Її виконання може тривати деякий час.
Перед виконанням команди відновлення необхідно зупинити сервіс
```console
sudo ./flibgolite -service stop
```
і після відновлення знову запустити
```console
sudo ./flibgolite -service start
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

### _7. Запуск у контейнері Docker_

Крім того, ви можете запустити програму в [контейнері docker](docs/docker.md).

### _8. Збірка з вихідних текстів_

Якщо у вас є сумніви щодо безпеки готових виконуваних файлів або не підходить для вашого обладнання та операційної системи, ви можете легко створити його самостійно.    
Щоб створити виконуваний файл, встановіть [Go] (https://go.dev / dl/), [Git] (https://git-scm.com/downloads), клонуйте [репозиторій FLibGoLite] (https://github.com/vinser/flibgolite) і запустіть
```консоль
go build ./cmd/flibgolite
```  
Краще зібрати його на тому хості, на якому буде запущена служба. Ви отримаєте виконуваний файл, придатний для операційної системи та апаратного забезпечення хоста.    
  
Для перехресної компіляції встановіть GNU `make` і запустіть його з Makefile

-------------------------------
___*Пропозиції, повідомлення про помилки та коментарі вітаються [тут](https://github.com/vincer/flibgolite/issues)*___
   

