---
author: Serguei Vine
title: Using Docker
description: Run FLibGoLite in Docker container
lastmod: 2025-01-03
slug: using-docker
layout: docs
---

If you want to install __FLibGoLite__ on a NAS based on open solutions such as openmediavault, Unraid, TrueNAS SCALE, Rockstor or commercial Synology, QNAP, etc., you can use a `docker container`. A ready-made container {{< link docker_hub >}}vinser/flibgolite:latest{{< /link >}} can be downloaded from DockerHub. To run it, follow the documentation of your NAS. The parameters for running the container are given below in the `docker_run.sh` script.  

Alternatively, you can run __FLibGoLite__ in a `docker container` on your host (PC, server) with Windows, MacOS or Linux OS.
To do this, the host must have {{< link docker_desktop >}}Docker Desktop{{< /link >}} or {{< link docker_engine >}}Docker Engine{{< /link >}} installed.  

#### Starting a docker container

To start the container, you need to create three folders:
1. The folder where you will put the books
2. The folder for the logs, where you can view information about the service's operation
3. The folder for the SQLite database files  

When starting the container, you must specify the paths to the created folders.
If necessary, you can specify the desired listening port number in the launch command `-p 8085:8085`.
After that, you can run the command to launch the container.

To perform the listed actions and launch the container, you can use the script `docker_run.sh`, which you can copy below

{{< file path="./assets/files/docker_run.sh" id="flibgolite-docker-run" lang="bash" full="false" >}}  

####  Using docker container

The script creates a `books` folder in the current directory. Place your books in this folder and they will be included in the OPDS catalog. The logs and SQLite database will be in the `logs` and `dbdata` folders, respectively.
> [!TIP]
> If you do not make backups of the SQLite database, then if problems occur due to power outages or a host shutdown, you need to do the following to restore the database: stop the container, clear the `dbdata` folder and start the container again. This will not take much time.  

#### Creating a Docker container image yourself

If you have any doubts about the safety of using a ready-made Docker container image, you can create it yourself directly on the host where Docker is installed.
To do this, you need to clone the flibgolite repository
```bash
git clone https://github.com/vinser/flibgolite.git
```
then create an image using the command
```bash
docker build -f Dockerfile.selfbuild -t flibgolite .
```
