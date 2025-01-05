---
author: Serguei Vine
title: Introduction
description: FLibGoLite is your home cloud library and more
lastmod: 2025-01-05
slug: introduction
layout: docs
---

Organize your cloud home library with __FLibGoLite__ and dust off e-readers

__FLibGoLite__ is an easy-to-use lightweight home library server with OPDS catalog that you can install on your PC, NAS or server.  

> [!NOTE]
> The OPDS catalog format is a syndication format for electronic publications based on Atom and HTTP. OPDS catalogs enable the aggregation, distribution, discovery, and acquisition of electronic publications. {{< link "wiki_opds" >}}(Wikipedia){{< /link >}}

__FLibGoLite__:
- Supports the publication format {{< link wiki_epub >}}EPUB{{< /link >}} and {{< link wiki_fb2 >}}FB2 (separate files and zip archives){{< /link >}}. It is possible to convert the FB2 format to EPUB when uploading a book to the reader.
- Self-sufficient - does not require installation of any additional libraries or applications.
- Multiplatform - can work in different operating systems `Linux`, `Windows`, `MacOS`, `FreeBSD` and processors of various architectures. Can be launched as a system service or in a `Docker` container, as well as from the command line.
- Shows high speed of processing new arrivals and storing the catalog in the {{< link sqlite_home >}}SQLite{{< /link >}} database. Even on weak hardware, the server can serve a collection of books comparable in size to {{< link project_gutenberg >}}Project Gutenberg{{< /link >}} and more.
- Has a fast and responsive OPDS service. The OPDS catalog has been tested and works with mobile applications for reading books `PocketBook Reader`, `FBReader`, `Librera Reader`, `Cool Reader`, as well as desktop applications `Foliate` and `Thorium Reader`. You can use any other applications or e-ink devices that can read the listed book formats and work with OPDS catalogs.  

___{{< link "docs/guide" >}}Create your own cloud library{{< /link >}}___, which will be used by your loved ones from smartphones, e-readers or PCs.  

__Current stable release is [{{% latest-tag %}}]({{% ref releases %}})__