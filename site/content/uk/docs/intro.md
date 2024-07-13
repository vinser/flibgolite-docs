---
author: Serguei Vine
title: Вступ
description: FLibGoLite - ваша домашня хмарна бібліотека і не тільки
date: 2024-02-21
slug: вступ
layout: docs
---
<h3 class="text-center">Створіть власну хмарну бібліотеку <b>FLibGoLite</b> і змахніть пилюку з читалок</h3>

__FLibGoLite__ — це простий у використанні легкий сервер домашньої бібліотеки з каталогом OPDS, який можна встановити на свій ПК, NAS або сервер.  

{{< alert type="info" >}}
Формат каталогу OPDS – це формат розповсюдження електронних публікацій на основі Atom та HTTP. Каталоги OPDS дозволяють агрегувати, розповсюджувати, виявляти та купувати електронні публікації. [(Вікіпедія)](https://en.wikipedia.org/wiki/Open_Publication_Distribution_System)
{{< /alert >}}

__FLibGoLite__ є мультиплатформним і може працювати на Windows, MacOs та Linux. Він використовує базу даних SQLite для індексу пошуку книг.

__FLibGoLite__ підтримує формат публікацій [EPUB](https://en.wikipedia.org/wiki/EPUB) та [FB2 (окремі файли та zip-архіви)](https://github.com/gribuser/fb2).

Каталог __FLibGoLite__ OPDS протестований та працює з мобільними програмами для читання книг __PocketBook Reader__, __FBReader__, __Librera Reader__, __Cool Reader__, а також настільними програмами __Foliate__ та __Thorium Reader__. Ви можете використовувати будь-які інші програми або пристрої e-ink, які можуть читати перелічені формати книг і працювати з каталогами OPDS.  

__Поточний стабільний випуск - [{{% latest %}}]({{<ref releases >}})__