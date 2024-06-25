---
author: Serguei Vine
title: Advanced User Guide
description: Advanced setup and fine tuning
date: 2024-06-12
slug: advanced-usage
layout: docs
---

## Advanced usage
From command line run `./flibgolite -help` to see run options
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

Examples:
{{< nav id="links-1" type="tabs" fade="true" >}}
{{< nav-item header="OS:" disabled=true />}}

{{< nav-item header="Windows" show="true" >}}
Open Powershell as Administrator and run commands
{{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
flibgolite                     ##Run FLibGoLite in console mode

flibgolite -service install    ##Install FLibGoLite as a system service
flibgolite -service start      ##Start FLibGoLite service	
{{</* /command */>}}

{{< /nav-item >}}
{{< nav-item header="macOS" >}}
Open terminal and run commands using `sudo`:
{{</* command user="user" host="localhost" */>}}
./flibgolite                          #Run FLibGoLite in console mode

sudo ./flibgolite -service install    #Install FLibGoLite as a system service
sudo ./flibgolite -service start      #Start FLibGoLite service	
{{</* /command */>}}

{{< /nav-item >}}
{{< nav-item header="Linux" >}}
Open terminal and run commands using `sudo`:
{{</* command user="user" host="localhost" */>}}
./flibgolite                          #Run FLibGoLite in console mode

sudo ./flibgolite -service install    #Install FLibGoLite as a system service
sudo ./flibgolite -service start      #Start FLibGoLite service	
{{</* /command */>}}

{{< /nav-item >}}
{{< /nav >}}

## Setup and fine tuning

### _1. Main configuration file_

Selfexplanatory configuration file `config.yml` with folder tree is created at the first programm run. This file is stored in the `config` subfolder of the program location. For advanced sutup you can edit it.  
Default content is as follows:
{{< file path="./assets/files/config.yml" id="flibgolite-config" full="false" show="false" >}}

### _2. Setting folders location_

To change location of a folder just edit corresponding line in `config.yml`
For example, if you need to setup separate folder for new aquired books uncomment line  
```yml
NEW: "books/new"
``` 
and change `books/new` to the appropriate folder path.

### _3. OPDS tuning_

You can change OPDS default 8085 http port to yours 
```yml
# OPDS-server port so opds can be found at http://<server name or IP-address>:8085/opds
PORT: 8085
```
Here you can set your desired OPDS server name
```yml
# OPDS-server title that is displayed in a book reader
TITLE: "FLib Go Go Go!!!"
```
You can change the number of books your bookreader will load at a time when you page (pulldown/update the screen)
```yml
# OPDS feeds entries page size
PAGE_SIZE: 30
```
Do not set this value more than default. With lower values it updates faster.

### _4. Localization tips_

For additional localization there are several simple settings.

4.1. By default new books processing is limited to English, Russian and Ukrainian books. You can add [others](https://en.wikipedia.org/wiki/IETF_language_tag) like `"de"`, `"fr"`, `"it"` and so on.  

```yml
# Accept only these languages publications. Add others if needed please.
ACCEPTED: "en, ru, uk"
```  

4.2. By default bookreader will show menues and comments in English `"en"` If you want in Russian or Ukranian you can change this setting to `"ru"` or `"uk`" 

```yml
# Default english locale for opds feeds (bookreaders opds menu tree) can be changed to:
# "uk" for Ukrainian, 
# "ru" for Russian 
DEFAULT: "en"
```

4.3. If your native language is other then three mentioned above for your convinience you can make language file and put it in `config/locales` folder  

```yml
# Locales folder. You can add your own locale file there like en.yml, ru.yml, uk.yml
DIR: "config/locales"
```

For example, for German, copy `en.yml` to `de.yml` and translate the phrases into German to the right of the colon separator. Leave `%d` format symbols untouchced. Something like this:  

```yml
Found authors - %d: Autoren gefunden - %d
```

Don't forget to replace alphabet string `ABC` to German. This will ensure that German names and titles are displayed and sorted correctly.  

4.4. Genres tree selection language adaptation can be done by editing the file `genres.xml` in `config` folder

```yml
  TREE_FILE: "config/genres.xml"
```

This can be done by adding language specific lines in `genres.xml` file

```xml
<genre-descr lang="en" title="Alternative history"/>
<genre-descr lang="ru" title="Альтернативная история"/>
<genre-descr lang="uk" title="Альтернативна історія"/>
<genre-descr lang="de" title="Alternative Geschichte"/>
```

### _5. Books index database_

Books index is stored in SQLite database file located in `dbdata` folder. It is created at the first program run and __is not intended for manual editing__

```yml
DSN: "dbdata/books.db"
```
If it is lost or damaged, it can be restored by the command
```console
sudo ./flibgolite -reindex
```
It may take some time to complete.
Before executing the recovery command, you must stop the service
```console
sudo ./flibgolite -service stop
```
and after recovery run again
```console
sudo ./flibgolite -service start
```

### _6. Logging_

While running service writes `opds.log` and `scan.log` located in `logs` folder.

```yml
OPDS: "logs/opds.log"
SCAN: "logs/scan.log"
```
`opds.log` contains records about bookreaders requests.  
`scan.log` contains records about new books and archive indexing.  
To redirect the log output to console (stdout) just comment out the appropriate line OPDS or SCAN.

You don't need to delete logs to free up disk space, as logs are rotated (overwrite) after 7 days.

You can setup logging level (verbosity) to one of: `D` - debug, `I` - info, `W` - warnings (default), `E` - errors
```yml
LEVEL: "W" 
```

### _7. Run in Docker container_

As an option you may run program in [docker container](docs/docker.md).

### _8. Build from sources_

If you have any security doubts about prebuilded executables or there is no suitable one for your hardware and OS you may easily build it yourself.    
To build an executable install [Go](https://go.dev/dl/), [Git](https://git-scm.com/downloads), clone [FLibGoLite repository](https://github.com/vinser/flibgolite) and run
```console
go build ./cmd/flibgolite
```  
It's better to build it on the host the service will run. You will get executable right for the host OS and hardware.    
  
For crosscompile install GNU `make` and run it with Makefile

-------------------------------
___*Suggestions, bug reports and comments are welcome [here](https://github.com/vinser/flibgolite/issues)*___

   

