---
author: Serguei Vine
title: Build from sources
description: Build FLibGoLite executable from sources
lastmod: 2024-06-12
slug: build-from-sources
layout: docs
---

If you have any security doubts about prebuilt executables or there is no suitable one for your hardware and OS you may easily build it yourself.    

To build an executable install {{< link golang_install >}}Go{{< /link >}}, {{< link git_install >}}Git{{< /link >}}, clone {{< link repository >}}FLibGoLite repository{{< /link >}}  and run
```console
go build ./cmd/flibgolite
```  
It's better to build it on the host the service will run. You will get executable right for the host OS and hardware.    
  
For crosscompile install GNU `make` and run it with Makefile