---
author: Serguei Vine
title: User Guide
description: How to download, install and use FLibGoLite
lastmod: 2024-08-25
slug: user-guide
layout: docs
---

__FLibGoLite__ program is written in GO as a single executable and doesn't require any prerequisites.  

__All you have to do is to download, install and start it.__

##  Download
Download program executable of the {{< latest-url >}}current release {{</* latest-tag */>}}{{< /latest-url >}} for your OS and CPU type  

{{< table class="table-striped" >}}
|OS      |CPU type              |Program executable          |Tested[^1]|
|--------|----------------------|----------------------------|:------:|  
|Windows | Intel, AMD 64-bit    | flibgolite-linux-amd64.exe |Yes     |  
|macOS   | Intel, AMD 64-bit    | flibgolite-darwin-amd64    |No      |  
|macOS   | ARM 64-bit           | flibgolite-darwin-arm64    |No      |  
|Linux   | Intel, AMD 64-bit    | flibgolite-linux-amd64     |No      |  
|Linux   | ARM 32-bit (armhf)   | flibgolite-linux-arm-6     |Yes     |  
|Linux   | ARM 64-bit (armv8)   | flibgolite-linux-arm64     |Yes     |  
{{< /table >}}

_[^1]: Some of executables was only cross-builded and not tested on real computers, but you can still try them out_  
{.small}

You may rename downloaded program executable to `flibgolite` or any other name you wish. For convenience, the name `flibgolite` will be used throughout the documentation.  
Place the executable `flibgolite` in any folder where you have read, write and execute permissions.

## Install and start
Although __FLibGoLite__ program can be run from command line, the preferred setup is program to be installed as a system service running in background that will automatically start after power on or reboot.

Before use, you need to complete three steps: __install__, __start__, and __check the status__ of the service.  
Service installation and control requires administrator privileges.   

{{< nav id="links-1" type="tabs" fade="true" >}}
{{< nav-item header="OS:" disabled=true />}}

{{< nav-item header="Windows" show="true" >}}
Open Powershell as Administrator and run commands
{{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
  flibgolite -service install
  flibgolite -service start
  flibgolite -service status
{{</* /command */>}}
{{< /nav-item >}}

{{< nav-item header="macOS" >}}
Open terminal and run commands using `sudo`:
{{</* command user="user" host="localhost" shell="bash" */>}}
  sudo ./flibgolite -service install
  sudo ./flibgolite -service start
  sudo ./flibgolite -service status
{{</* /command */>}}
{{< /nav-item >}}

{{< nav-item header="Linux" >}}
Open terminal and run commands using `sudo`:
{{</* command user="user" host="localhost" shell="bash" */>}}
  sudo ./flibgolite -service install
  sudo ./flibgolite -service start
  sudo ./flibgolite -service status
{{</* /command */>}}
{{< /nav-item >}}

{{< /nav >}}

If status is like `running` you can start to use it.

## Use
At the first run program will create the set of subfolders in the folder where program is located  
```console
flibgolite
├─ books  
|  ├─ stock     - library book files and archives are stored here
|  └─ trash     - files with processing errors will go here
├─ config       - contains main configuration file config.yml and genre tree file
|  └─ locales   - subfolder for localization files 
├─ dbdata       - database with book index resides here
└─ logs         - scan and opds rotating logs are here

```

Put your book files or book file zip-archives in `books/stock` folder and start to setup bookreader. Meanwhile book descriptions will be added to book index of OPDS-catalog.

Set bookreader opds-catalog url to `http://<PC_name or PC_IP_address>:8085/opds` to choose and download books on your device to read. See bookreader manual/help.

> [!NOTE]
> While searching book in bookreader use native keyboard layout for chosen language to fill search pattern. For example, don't use Latin English "i" instead of Cyrillic Ukrainian "i", because it's not the same Unicode symbol. 