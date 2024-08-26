---
author: Serguei Vine
title: Introduction
description: FLibGoLite is your home cloud library and more
lastmod: 2024-08-25
slug: introduction
layout: docs
---

Organize your cloud home library with __FLibGoLite__ and dust off e-readers

__FLibGoLite__ is an easy-to-use lightweight home library server with {{< abbr OPDS >}} catalog that you can install on your PC, NAS or server.  

> [!NOTE]
> The OPDS catalog format is a syndication format for electronic publications based on Atom and HTTP. OPDS catalogs enable the aggregation, distribution, discovery, and acquisition of electronic publications. {{< link "wiki_opds" >}}(Wikipedia){{< /link >}}

__FLibGoLite__ is multi-platform and can run on `Windows`, `macOS` and `Linux`. The catalog used by the `OPDS` server when searching and downloading books is stored in the {{< link sqlite_home >}}SQLite{{< /link >}} database. Thanks to this, even on weak hardware, it can serve a collection of books comparable to {{< link project_gutenberg >}}Project Gutenberg{{< /link >}} and larger.  

__FLibGoLite__ supports {{< link wiki_epub >}}EPUB{{< /link >}} and {{< link wiki_fb2 >}}FB2 (single files and zip archives){{< /link >}}  publication formats.

__FLibGoLite__ OPDS catalog has been tested and works with mobile book reader applications `PocketBook Reader`, `FBReader`, `Librera Reader`, `Cool Reader`, as well as desktop applications `Foliate` and `Thorium Reader`. You can use any other applications or e-ink devices that can read the listed book formats and work with OPDS catalogs.

__Current stable release is [{{% latest-tag %}}]({{% ref releases %}})__