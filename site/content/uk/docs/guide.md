---
author: Serguei Vine
title: Інструкція користувача
description: Як встановити та використовувати FLibGoLite
lastmod: 2025-02-22
slug: інструкція-користувача
layout: docs
---

Програма __FLibGoLite__ написана на GO як один виконуваний файл і не вимагає пререквізитів.  

__Все, що вам потрібно зробити, це завантажити, встановити та запустити ee.__

### Завантаження
Завантажте виконуваний файл програми {{< latest-url >}}поточної версії {{</* latest-tag */>}}{{< /latest-url >}} для вашої ОС та типу процесора.  

{{< table class="table-striped" >}}
|ОС      |Процессор           |Виконуваний файл            {{</* sup 1 */>}}|
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

_{{< sup 1 >}} Деякі файли були лише перехресно зібрані і не тестувалися на реальних комп'ютерах, але ви все одно можете їх випробувати._  
{.small}

Ви можете перейменувати виконуваний файл програми в `flibgolite` або будь-яке інше ім'я за вашим бажанням. Далі в документації для зручності буде використовуватися ім'я `flibgolite`.  
Помістіть файл `flibgolite` в будь-яку папку, в якій у вас є права на читання, запис та виконання.

### Встановлення та запуск

Незважаючи на те, що програму __FLibGoLite__ можна запускати з командного рядка, кращою є установка програми як системна служба, що працює у фоновому режимі, яка автоматично запускається після включення живлення або перезавантаження.

Перед використанням необхідно виконати три кроки: __встановити__, __запустити__ і __перевірити статус служби.
> [!IMPORTANT]
> Для встановлення та керування службою потрібні права адміністратора.

{{< nav id="links-1" type="tabs" fade="true" >}}
{{< nav-item header="OS:" disabled=true />}}

{{< nav-item header="Windows" show="true" >}}
Відкрийте Powershell від імені адміністратора та виконайте команди.
{{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
  flibgolite.exe -service install
  flibgolite.exe -service start
  flibgolite.exe -service status
{{</* /command */>}}
{{< /nav-item >}}

{{< nav-item header="macOS, Linux, FreeBSD" >}}
Відкрийте термінал і виконайте команди за допомогою sudo:
{{</* command user="user" host="localhost" shell="bash" */>}}
  sudo ./flibgolite -service install
  sudo ./flibgolite -service start
  sudo ./flibgolite -service status
{{</* /command */>}}
{{< /nav-item >}}

{{< /nav >}}
Якщо статус схожий на `running`, ви можете почати використовувати сервіс.

### Використання
---

При першому запуску програма створить набір папок у папці, де знаходиться програма.  
```console
flibgolite
├─ books  
|  └─ stock     - тут зберігаються файли та архіви книг бібліотеки
├─ config       - містить основний файл конфігурації config.yml та файл дерева жанрів
|  └─ locales   - підпапка для файлів локалізації 
├─ dbdata       - база даних із покажчиком книг знаходиться тут
└─ logs         - тут знаходяться журнали сканування та запитів

```
Помістіть файли книг та zip-архіви файлів книг у папку `books/stock` та приступайте до налаштування вашої читалки. Файли в `books/stock` можна копіювати і при сервісі, що працює. Сервіс перевіряє, що файл ще не готовий і чекає, поки скопіюється.  

> [!WARNING]
> Не використовуйте підпапки в папці `stock`. Поточна версія __FLibGoLite__ може використовувати лише книги у форматах `EPUB`, `FB2` та `zip-архіви з книгами FB2`.

Поки описи книг додаються та стають доступними у вашому каталозі OPDS, ви можете скористатися посібником/інструкціями до читалки та налаштувати на ній URL для OPDS каталогу - `http://<ім'я_ПК або IP_адреса_ПК>:8085/opds`. Після цього ви зможете вибирати книги з каталогу та завантажувати їх у читалку.  

> [!NOTE]
> Під час пошуку книги в читалці використовуйте рідну розкладку клавіатури для вибраної мови, щоб заповнювати рядок пошуку. Наприклад, не використовуйте латинську англійську літеру «i» замість кириличної української літери «i», оскільки це не той самий символ Юнікод.