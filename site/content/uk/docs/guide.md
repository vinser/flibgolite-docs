---
author: Serguei Vine
title: Інструкція користувача
description: Як встановити та використовувати FLibGoLite
lastmod: 2024-08-25
slug: інструкція-користувача
layout: docs
---

Програма __FLibGoLite__ написана на GO як один виконуваний файл і не вимагає пререквізитів.  

__Все, що вам потрібно зробити, це завантажити, встановити та запустити ee.__

### Завантаження
Завантажте виконуваний файл програми {{< latest-url >}}поточної версії {{</* latest-tag */>}}{{< /latest-url >}} для вашої ОС та типу процесора.  

{{< table class="table-striped" >}}
|ОС        |Процессор             |Виконуваний файл            |Протестовано {{</* sup 1 */>}}|  
|----------|----------------------|----------------------------|:------------:|  
|Windows   | Intel, AMD 64-bit    | flibgolite-linux-amd64.exe |Так           |  
|macOS     | Intel, AMD 64-bit    | flibgolite-darwin-amd64    |Ні            |  
|macOS     | ARM 64-bit           | flibgolite-darwin-arm64    |Ні            |  
|Linux     | Intel, AMD 64-bit    | flibgolite-linux-amd64     |Ні            |  
|Linux     | ARM 32-bit (armhf)   | flibgolite-linux-arm-6     |Так           |  
|Linux     | ARM 64-bit (armv8)   | flibgolite-linux-arm64     |Так           |  
{{< /table >}}

_{{< sup 1 >}} Деякі файли були лише перехресно зібрані і не тестувалися на реальних комп'ютерах, але ви все одно можете їх випробувати._  
{.small}

Ви можете перейменувати виконуваний файл програми в `flibgolite` або будь-яке інше ім'я за вашим бажанням. Далі в документації для зручності буде використовуватися ім'я `flibgolite`.  
Помістіть файл `flibgolite` в будь-яку папку, в якій у вас є права на читання, запис та виконання.

### Встановлення та запуск

Незважаючи на те, що програму __FLibGoLite__ можна запускати з командного рядка, кращою є установка програми як системна служба, що працює у фоновому режимі, яка автоматично запускається після включення живлення або перезавантаження.

Перед використанням необхідно виконати три кроки: __встановити__, __запустити__ і __перевірити статус служби.
Для встановлення та керування службою потрібні права адміністратора.

{{< nav id="links-1" type="tabs" fade="true" >}}
{{< nav-item header="OS:" disabled=true />}}

{{< nav-item header="Windows" show="true" >}}
Відкрийте Powershell від імені адміністратора та виконайте команди.
{{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
  flibgolite -service install
  flibgolite -service start
  flibgolite -service status
{{</* /command */>}}
{{< /nav-item >}}

{{< nav-item header="macOS" >}}
Відкрийте термінал і виконайте команди за допомогою sudo:
{{</* command user="user" host="localhost" shell="bash" */>}}
  sudo ./flibgolite -service install
  sudo ./flibgolite -service start
  sudo ./flibgolite -service status
{{</* /command */>}}
{{< /nav-item >}}

{{< nav-item header="Linux" >}}
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
|  ├─ stock     - тут зберігаються файли та архіви книг бібліотеки
|  └─ trash     - сюди потраплятимуть файли з помилками обробки
├─ config       - містить основний файл конфігурації config.yml та файл дерева жанрів
|  └─ locales   - підпапка для файлів локалізації 
├─ dbdata       - база даних із покажчиком книг знаходиться тут
└─ logs         - тут знаходяться журнали сканування та запитів

```
Помістіть файли книг або zip-архіви файлів книг у папку «books/stock» і приступайте до налаштування читалки в той час, як описи книг будуть додані до індексу каталогу OPDS.
Встановіть для opds-каталога url `http://<ім'я_ПК або IP_адреса_ПК>:8085/opds`, щоб вибирати та завантажувати книги на свій пристрій для читання. Дивись посібник/довідку до читалки.

> [!NOTE]
> Під час пошуку книги в читалці використовуйте рідну розкладку клавіатури для вибраної мови, щоб заповнювати рядок пошуку. Наприклад, не використовуйте латинську англійську літеру «i» замість кириличної української літери «i», оскільки це не той самий символ Юнікод.