---
author: Serguei Vine
title: Руководство пользователя
description: Как установить и использовать FLibGoLite
lastmod: 2025-02-22
slug: руководство-пользователя
layout: docs
---

Программа __FLibGoLite__ написана на GO как один исполняемый файл и не требует каких-либо пререквизитов.  

__Все, что вам нужно сделать, это загрузить, установить и запустить еe.__

###  Загрузка
Загрузите исполняемый файл программы {{< latest-url >}}текущей версии {{</* latest-tag */>}}{{< /latest-url >}} для вашей ОС и типа процессора.
  
{{< table class="table-striped" >}}
|ОС      |Процессор             |Исполняемый файл            {{</* sup 1 */>}}|
|--------|----------------------|------------------------------|  
|Windows | Intel, AMD 64-bit    | flibgolite-windows-amd64.exe |  
|Windows | Intel, 32-bit        | flibgolite-windows-386.exe   |  
|macOs   | Intel, AMD 64-bit    | flibgolite-darwin-amd64      |  
|macOS   | ARM 64-bit           | flibgolite-darwin-arm64      |  
|Linux   | Intel, AMD 64-bit    | flibgolite-linux-amd64       |  
|Linux   | Intel, 32-bit        | flibgolite-linux-386         |  
|Linux   | ARM 32-bit (armV6)   | flibgolite-linux-arm-6       |  
|Linux   | ARM 32-bit (armV7)   | flibgolite-linux-arm-7       |  
|Linux   | ARM 64-bit           | flibgolite-linux-arm64       |  
|FreeBSD | Intel, AMD 64-bit    | flibgolite-freebsd-amd64     |  
|FreeBSD | Intel, 32-bit        | flibgolite-freebsd-386       |  
|FreeBSD | ARM 32-bit (armV6)   | flibgolite-freebsd-arm-6     |  
|FreeBSD | ARM 32-bit (armV7)   | flibgolite-freebsd-arm-7     |  
|FreeBSD | ARM 64-bit           | flibgolite-freebsd-arm64     |  
{{< /table >}}

_{{< sup 1 >}} Некоторые исполняемые файлы были только перекрестно собраны и не тестировались на реальных компьютерах, но вы все равно можете их опробовать._  
{.small}

Вы можете переименовать исполняемый файл программы во `flibgolite` или в любое другое имя по вашему желанию. Для удобства далее в документации будет использоваться имя `flibgolite`.  
Поместите исполняемый файл `flibgolite` в любую папку, в которой у вас есть права на чтение, запись и исполнение.  

### Установка и запуск
Не смотря на то, что программу __FLibGoLite__ можно запускать из командной строки, предпочтительной является установка программы в качестве системной службы, работающей в фоновом режиме, которая автоматически запускается после включения питания или перезагрузки.

Перед использованием необходимо выполнить три шага: __установить__, __запустить__ и __проверить статус__ службы.
> [!IMPORTANT]
> Для установки и управления службой требуются права администратора.

{{< nav id="links-1" type="tabs" fade="true" >}}
{{< nav-item header="OS:" disabled=true />}}

{{< nav-item header="Windows" show="true" >}}
Откройте Powershell от имени администратора и выполните команды.
{{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
  flibgolite.exe -service install
  flibgolite.exe -service start
  flibgolite.exe -service status
{{</* /command */>}}
{{< /nav-item >}}

{{< nav-item header="macOS, Linux, FreeBSD" >}}
Откройте терминал и выполните команды, используя sudo:
{{</* command user="user" host="localhost" shell="bash" */>}}
  sudo ./flibgolite -service install
  sudo ./flibgolite -service start
  sudo ./flibgolite -service status
{{</* /command */>}}
{{< /nav-item >}}

{{< /nav >}}
Если статус похож на `running`, вы можете начать использовать сервис.

### Использование
При первом запуске программа создаст набор подпапок в папке, где находится программа.

```console
flibgolite
├─ books  
|  └─ stock   - здесь хранятся файлы и архивы книг библиотеки
├─ config     - содержит основной файл конфигурации config.yml и файл дерева жанров
|  └─ locales - подпапка для файлов локализации 
├─ dbdata     - база данных с указателем книг находится здесь
└─ logs       - здесь находятся журналы сканирования и запросов

```
Поместите файлы книг и zip-архивы файлов книг в папку `books/stock` и приступайте к настройке вашей читалки. Файлы в `books/stock` можно коприровать и при работающем сервисе. Сервис проверяет, что файл еще не готов и ждёт пока скопируется.

> [!WARNING]
> Не используйте подпапки в папке `stock`. Текущая версия __FLibGoLite__ может использовать только книги в форматах `EPUB`, `FB2` и `zip-архивы с книгами FB2`.

Пока описания книг добавляются и становятся доступными в вашем каталоге OPDS, вы можете воспользоваться руководством/инструкциями к читалке и настроить на ней URL для OPDS каталога - `http://<имя_ПК или IP_адрес_ПК>:8085/opds`. После этого вы сможете выбирать книги из каталога и загружать их в читалку.  

> [!NOTE]
> При поиске книги в читалке используйте родную раскладку клавиатуры для выбранного языка, чтобы заполнять строку поиска. Например, не используйте латинскую английскую букву «i» вместо кириллической украинской буквы «i», поскольку это не один и тот же символ Юникод.
