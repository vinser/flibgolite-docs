---
author: Serguei Vine
title: Advanced User Guide
description: Advanced setup and fine tuning
lastmod: 2025-01-06
slug: advanced-usage
layout: docs
---

## Run options

To view the run options, type `./flibgolite -help` at the command prompt
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
{{< nav id="tabs-1" type="tabs" fade="true" >}}

  {{< nav-item header="OS:" disabled=true />}}

  {{< nav-item header="Windows" show="true" >}}
    Open Powershell as Administrator and run commands
    {{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
      flibgolite                     ##Run FLibGoLite in console mode

      flibgolite -service install    ##Install FLibGoLite as a system service
      flibgolite -service start      ##Start FLibGoLite service	
    {{</* /command */>}}
  {{< /nav-item >}}

  {{< nav-item header="macOS, Linux, FreeBSD" >}}
    Open terminal and run commands using `sudo`
    {{</* command user="user" host="localhost" */>}}
      ./flibgolite                          ##Run FLibGoLite in console mode

      sudo ./flibgolite -service install    ##Install FLibGoLite as a system service
      sudo ./flibgolite -service start      ##Start FLibGoLite service	
    {{</* /command */>}}
  {{< /nav-item >}}

{{< /nav >}}

## Fine tuning

### _1. Main configuration file_

Self-explanatory configuration file `config.yml` in folder tree is created at the first program run. This file is stored in the `config` subfolder of the program location. For advanced setup you can edit it.  
Default content is as follows:
{{< file path="./assets/files/config.yml" id="flibgolite-config" full="false" show="false" >}}

### _2. Setting book folder location_

By default, FLibGoLite adds books from the `books/stock` folder to the OPDS catalog and does not move them anywhere. To change the folder location, simply edit the corresponding line in `config.yml`
```yml
  STOCK: "books/stock" # Book stock
``` 
and change `books/stock` to the appropriate folder path.  
>[!CAUTION]
>In paths, use forward slash `/` instead of backslash `\`.

You can also do this by creating a link to another folder on the disk where your books are located.  
For example:
{{< nav id="tabs-2" type="tabs" fade="true" >}}

{{< nav-item header="OS:" disabled=true />}}

{{< nav-item header="Windows" show="true" >}}
Open Powershell and run the command to create a directory junction
{{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
mklink /J .\books D:\path\to\books
{{</* /command */>}}
{{< /nav-item >}}

{{< nav-item header="macOS, Linux, FreeBSD" >}}
Open Terminal and run the command to create a symbolic link
{{</* command user="user" host="localhost" */>}}
ln -s /path/to/books ./books
{{</* /command */>}}
{{< /nav-item >}}

{{< /nav >}}
### _3. OPDS tuning_

3.1. You can change OPDS default 8085 http port to yours 
```yml
# OPDS-server port so opds can be found at http://<server name or IP-address>:8085/opds
PORT: 8085
```
3.2. Here you can set your desired OPDS server name
```yml
# OPDS-server title that is displayed in a book reader
TITLE: "FLib Go Go Go!!!"
```
3.3. You can change the number of books your bookreader will load at a time when you page (pulldown/update the screen)
```yml
# OPDS feeds entries page size
PAGE_SIZE: 20
```
>[!WARNING]
>Do not set this value significantly higher than the default. This may make page refreshing or scrolling uncomfortable.

3.4. You can disable on the fly FB2 to EPUB format conversion when downloading books to the e-reader, just uncomment line to set NO_CONVERSION to `true`
```yml
# Do not convert FB2 to EPUB format
NO_CONVERSION: true
```

### _4. Localization tips_

For additional localization there are several possible settings.

4.1. By default new books processing is limited to English, Russian and Ukrainian books. You can expand this list by adding the necessary {{< link ietf_language_tags >}}IETF tags{{< /link >}}, such as `de`, `fr`, `it`, etc.  

```yml
# Accept only these languages publications. Add others if needed please. Set to "any" to accept any language 
ACCEPTED: "en, ru, uk"
```  
Choose `any` to accept books in any language
```yml
ACCEPTED: "any"
```   

4.2. By default bookreader will show menus and comments in English `en` If you want in Russian or Ukrainian you can change this setting to `ru` or `uk` 

```yml
# Default english locale for opds feeds (bookreaders opds menu tree) can be changed to:
# "uk" for Ukrainian, 
# "ru" for Russian 
DEFAULT: "en"
```
If in the first request to the OPDS catalog the bookreader sends the `Accept-Language` header with the language specification, then the requested language will be set as default.

>[!NOTE]  
>To leave the only one language, you can delete all locale files in the `config/locales` folder except for the selected default language. In this case, the reader will not display the language selection menu. See Section 4.3 about locale files.

4.3. If your native language is other then three mentioned before for your convenience you can make your own locale file and put it in `config/locales` folder.  

```yml
# Locales folder. You can add your own locale file there like en.yml, ru.yml, uk.yml
DIR: "config/locales"
```

For example, for German, copy `en.yml` to `de.yml` and translate the phrases into German to the right of the colon separator. Leave `%d` format symbols untouched. Something like this:  

```yml
Found authors - %d: Autoren gefunden - %d
```

Don't forget to replace alphabet string `ABC` to German. This will ensure that German names and titles are displayed and sorted correctly.  

4.4. Genres tree selection language adaptation can be done by editing the file `genres.xml` in `config` folder.

```yml
  TREE_FILE: "config/genres.xml"
```

This can be done by adding language specific lines in `genres.xml` file.

```xml
<genre-descr lang="en" title="Alternative history"/>
<genre-descr lang="ru" title="Альтернативная история"/>
<genre-descr lang="uk" title="Альтернативна історія"/>
<genre-descr lang="de" title="Alternative Geschichte"/>
```

### _5. Book index database_

5.1. Book index is stored in SQLite database files `books.db` located in `dbdata` folder. 
```yml
DSN: "dbdata/books.db"
```
> [!CAUTION]
> Database files are created at the first program run and __are not intended for manual editing__

5.2. If database files are lost or corrupted, they can be restored by reindexing the books stock folder
{{< nav id="tabs-2" type="tabs" fade="true" >}}
  {{< nav-item header="OS:" disabled=true />}}

  {{< nav-item header="Windows" show="true" >}}
    Open Powershell as Administrator and run command
    {{</* command prompt="PS C:\Users\User\flibgolite>" shell="powershell" */>}}
      flibgolite -reindex
    {{</* /command */>}}
  {{< /nav-item >}}

  {{< nav-item header="macOS, Linux, FreeBSD" >}}
    Open terminal and run command using `sudo`
    {{</* command user="user" host="localhost" */>}}
      sudo ./flibgolite -reindex
    {{</* /command */>}}
  {{< /nav-item >}}

{{< /nav >}}

It may take some time to complete.
>[!NOTE]  
>The same can be done manually: stop the service, delete the database files and start the service again.

5.3. The process of registering new acquisitions is presented in the diagram.
```goat
 .----------------------+
| STOCK folder scanning |
+------+---------------'
       |
       v                                                                                              
+------------------------+
| Scan every 300 seconds |
+------+-----------------+
       |                                                                                              
       v                                                                                           .---------. 
       |   +----------------------+       .-------------------+      +---------------------+      |           |   
       +-->| Parallel EPUB parser +--+-->| Parsed books queue +----->| Bulked Tx worker    +----->|'---------'|
       |   +----------------------+  |   |--------------------|      |---------------------|      |           |
       |                             |   | max 20000 books    |      | max 20000 at a time |      | SQLite DB |
       |                             |   +-------------------'       +---------------------+      |           |
       |                             |               ^                                             '---------' 
       |   +----------------------+  |               |                                                
       +-->| Parallel FB2 parser  +--+   +-----------+---------+
       |   +----------------------+      | parallel FB2 parser |
       |                                 |---------------------|
       |                                 |    10 workers       |
       |                                 +---------------------+
       |                                             ^ 
       |                                             |  
       |   +----------------------+       .----------+---------+
       +-->| Unzip FB2 zip-archive|----->| Unziped files queue |
           +----------------------+      |---------------------|
                                         | max 20000 files     | 
                                         +--------------------'
       
```
In most cases, the process of registering new acquisitions does not require configuration, since its speed is determined mainly by the performance of the input/output system (controllers, disk types, etc.). If necessary, you can change the operation of the process using the following settings:
```yml
  # Delay before start each new acquisitions folder processing
  POLL_DELAY: 300 
  # Maximum parallel new acquisitions processing routines
  MAX_SCAN_THREADS: 10
  # Book queue size
  BOOK_QUEUE_SIZE: 20000
  # File queue size
  FILE_QUEUE_SIZE: 20000
  # Maximum number of books in one transaction
  MAX_BOOKS_IN_TX: 20000
```

### _6. Logging_

While running service writes `opds.log` and `scan.log` located in `logs` folder.

```yml
  # Logs are here
  OPDS: "logs/opds.log"
  SCAN: "logs/scan.log"
```
`opds.log` contains records about bookreaders requests.  
`scan.log` contains records about new books and archive indexing.  

You don't need to delete logs to free up disk space, as logs are rotated (overwrite) after 7 days.

You can setup logging level (verbosity) to one of: 
- `D` - debug,
- `I` - info,
- `W` - warnings (default),
- `E` - errors
```yml
  # Logging levels: D - debug, I - info, W - warnings (default), E - errors
  LEVEL: "W" 
```

### _7. Authentication_

You can set up authentication on your OPDS server by setting the authentication method and a number of additional parameters.  

The current release implements only one authentication method. This is HTTP Basic authentication - authentication by username and password.    

To enable Basic authentication, you must specify the `plain` method and credentials `username:password` the `auth` section of the `config.yml` file.  
```yml
auth:
#   Set METHOD to plain for http basic authentication with plain text master credentials from config.yml
  METHOD: "plain"
#   and fill authentication credentials in plain text format username:password 
  CREDS: "username:password"
```
>[!CAUTION]
>The implemented `plain` method has security flaws because the password is stored in plaintext in the configuration file. In addition, when using basic authentication outside the home network without using the https protocol, credentials can be intercepted by attackers.  


-------------------------------
___*Suggestions, bug reports and comments are welcome {{< link repository_issues >}}here{{< /link >}}*___

   

