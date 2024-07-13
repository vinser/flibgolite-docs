---
author: Serguei Vine
title: Introduction
description: FLibGoLite is your home cloud library and more
date: 2024-02-21
slug: introduction
layout: docs
---
<h3 class="text-center">Organize your cloud home library with <b>FLibGoLite</b> and dust off e-readers</h3>

__FLibGoLite__ is an easy-to-use lightweight home library server with {{< abbr OPDS >}} catalog that you can install on your PC, NAS or server.  
{{< alert type="info">}}
The OPDS catalog format is a syndication format for electronic publications based on Atom and HTTP. OPDS catalogs enable the aggregation, distribution, discovery, and acquisition of electronic publications. {{</* link "wiki_opds" */>}}(Wikipedia){{</* /link */>}}
{{< /alert >}}

__FLibGoLite__ is multi-platform and can run on `Windows`, `macOS` and `Linux`. It uses `SQLite` database for book search index.

__FLibGoLite__ supports {{< link wiki_epub >}}EPUB{{< /link >}} and {{< link wiki_fb2 >}}FB2 (single files and zip archives){{< /link >}}  publication formats.

__FLibGoLite__ OPDS catalog has been tested and works with mobile book reader applications `PocketBook Reader`, `FBReader`, `Librera Reader`, `Cool Reader`, as well as desktop applications `Foliate` and `Thorium Reader`. You can use any other applications or e-ink devices that can read the listed book formats and work with OPDS catalogs.

__Current stable release is [{{% latest %}}]({{<ref releases >}})__