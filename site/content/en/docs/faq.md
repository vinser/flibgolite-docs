---
author: Serguei Vine
title: FAQ
description: Frequently Asked Questions and Tips
lastmod: 2025-01-06
slug: frequently-asked-questions
layout: docs
---

1. The installer for Windows is not visible from the iPad.
- To install as a service, you need to download the release build for the desired OS and host processor and run it on the host with the parameters as described in the {{< link href="docs/guide" >}}User Guide{{< /link >}}

2. Doesn't take off yet:
```bash
$ ./flibgolite-linux-amd64 -reindex
```
- To take off, you need to run without parameters or with the `-config` parameter if you want to correct the default configuration (see {{< link href="docs/advanced" >}}Advanced Guide{{< /link >}}).
- The `-reindex` parameter is used to force reindexing of the finished storage (DB) and is practically not needed.
- After setting up the configuration, I recommend installing flibgolite as a service, then it will work in the background and automatically start when you turn on or reboot your PC. No special knowledge is required for installation, as well as any third-party applications. There are ready-made executable files for Linux, Windows, macOS, FreeBSD. You can take {{< latest-url >}}here{{< /latest-url >}}.
- Since flibgolite is written in bare Go, those who want and know how can easily compile it from source texts for their OS

3. Tell me, did I understand correctly - you do not scan subdirectories?
- Yes, you are right, subdirectories are not scanned. Only EPUB, FB2 and ZIP archives with FB2 located directly in the directory specified in `STOCK` are processed

4. The log (`logs/scan.log`) contains a bunch of similar entries, a file name would be useful here, of course/
- By default, to keep the logs from getting bloated, the logging level logs/LEVEL is set to `W` - warnings and higher in `config.yml`. If you need more details, you can change it to `I` - info or `D` - debug

5. Are zip64 archives at least processed normally (for example, 3..4GB)?
- Yes, they are processed.

6. Why give the normal FB2 format as EPUB?
- EPUB is a normal international format, and FB2, no matter how good it is, is just a Runet format. It is unlikely that there is a reader that does not understand EPUB, which cannot be said about FB2. Some OPDS readers allow you to choose which format to download.
- FB2->EPUB conversion can be easily disabled by the setting in config.yml
```yml
# Do not convert FB2 to EPUB format
NO_CONVERSION: true
```

7. Where can I even look at the output interface of your program for web/opds? Unfortunately, it is simply not on the site.
- FLibGoLite does not have its own user interface, it is a service that outputs a catalog of books in OPDS format. Therefore, the interface in which the catalog is displayed depends on which reader is used. But the structure of the library catalog will be the same for all readers. Books in the catalog can be selected by author, genre, series. Or you can simply poke a magnifying glass, search by title and/or author and get quick relevant output.

8. I noticed that the program name is different...
- You can give the binary/executable any name. In Unix, you also need to set the execution permission for it.
The library itself, the name "FLib Go Go Go!!!", which is displayed in the reader, can be changed in config.yml.

9. It's strange that I don't have a search line in the library with the input of the title, author, etc. Only by iterating through the list.
- There is a contextual search by author and/or book title. It is usually in a separate field in the reader itself, usually at the top, next to the magnifying glass.

10. I'm wondering if I should make a hard link to the folder from the torrent folder to the stocs folder? So that books can be downloaded there from the torrent right away.
- The links work for example on the following systems:  
  - **Windows 10** - `mklink /J .\books D:\FGL\books`
  - **Ubuntu 24.04** - `ln -s /home/user/Dev/books books`
- You can also set up the desired folder in config.yml. When the service is running, you can copy files to stock. The service checks that the file is not ready yet and waits until it is copied.

11. How to update? Just replace the executable?
- For now, to update, you need to stop the service and replace the executable. If there are changes in the database, then delete the database - I warn you about this in major releases.
While there are no manual changes (web interface), I think that deleting the catalog database and creating a new catalog is faster than migrating taking into account contextual indexing.

12. How to check the number of books in the database?
- The number of books in the catalog can be viewed in the reader in the section "Language/Select book language". There are numbers there, how many books and in what language there arein the catalog.
You should also take into account that by default, books are not added to the catalog in all languages, see the documentation for how to add/remove a language.

13. But there are still fewer books visible than in other catalogs.
- FLibGoLite does not include duplicates in the catalog. There is a setting for three levels of duplicate rejection:

  - S - slow. CRC32 or Title + Description matches are checked.
  - F - fast. Only CRC32 matches are checked - by default
  - N - none. Duplicates are not rejected.

14. Do you have PDF support?
- Convert to PDF or add book information to the catalog in PDF format? 
  1. I don't plan to convert yet.
  2. For the second, I don't have an understanding of the metadata standard in PDF (author, title, ...), and if you enter it manually, there is no admin panel yet either.

15. I connect with two smartphones. On one, the first list contains authors and books in English. All the others are simply not there. For the rest, you need to go to the root section of languages ​​and further. On the second smartphone, everything is the same, but instead of English, there is Russian.
- For those readers that specify the desired language in the `Accept-Language` header in their request to the service, the service returns the first page for the requested language, and for those that do not use such a header, it shows the default language. If desired, you can set the default language, see Advanced Usage, p. 4.2.

16. How do I enable the "Books" section in the root section? i.e. the list of books
- It is not possible to enable the "Books" section on the first page - it does not exist. It is not clear what its purpose is - to sort books alphabetically?
There is only a full-text search in the catalog database (FTS5), which searches both titles and authors, which can return two groups: Books and Authors.
In each of the groups, the results are sorted in descending order of relevance, approximately like in the results of standard search engines.
And this is the main and fastest search in the catalog, if you roughly know the title or author.

- The logic is approximately as follows:
  - Need a specific book - use the search
  - Want to see a list of books by an author - search by author or in Authors alphabetically
  - Want to read something on a topic - search in genres
  - Want from a series - search in series
  - In the original language - select the language

17. If the library is on a laptop, should it be constantly on and online?
- No, only when you need to download a book from it.

18. I want to add a folder with subfolders to this library. Will the files be moved or renamed somewhere?
- The logic for working with subfolders has not yet been thought out, therefore, it has not been implemented.
Starting with the release of v2.2.0, the files are not moved. Everything is in the folder specified in `STOCK`.
By default, scanning starts 5 minutes (300 sec) after the end of the previous one.
If the library is not replenished often, you can set up scanning, for example, after a day (POLL_DELAY: 86400)

19. If the settings are ru, en, uk, the interface language floats between Russian and English.
- The ACCEPTED parameter: "en, ru, uk" is just a list of languages ​​for which information about books is entered into the catalog. This is not related to localization.
To set the interface language (OPDS tree) by default, there is a special option in the config
```yml
locales:
DEFAULT: "en"
```
in which the selected language must be one of the available localization files and have a corresponding translation in genres.xml
This is described in more detail in the English version of the documentation.

20. I launch PowerShell, but I can't enter these commands. It gives an error - flibgolite: The name "flibgolite" is not recognized...
- Briefly:
  1. Since you have Windows, download {{< latest-url >}}here{{< /latest-url >}} the file `flibgolite-windows-amd64.exe`.
  2. In your folder `C:\users\<your personal folder>` create a folder `flibgolite`. You can use Explorer.
  3. From the Downloads folder, move the file `flibgolite-windows-amd64.exe` to this `flibgolite` folder and rename it to `flibgolite.exe`
  4. Open PowerShell (administrator) and go to the created folder with the command `cd C:\users\<your personal folder>\flibgolite`
  5. And then, as written in the documentation, type the command `flibgolite -service install`

21. Another question, should there be `en.yml` file in the locales directory?
- When flibgolite is first launched, it creates several folders in the current folder, in particular, the config folder
```console
config
├── config.yml
├── genres.xml
└── locales
    ├── en.yml
    ├── ru.yml
    └── uk.yml
```
The locales folder must contain at least a localization file for the default language, for example, for `en` - `en.yml`