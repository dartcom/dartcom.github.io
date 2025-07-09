---
title: "Running PartDB With Podman"
date: 2025-07-09T20:27:17+03:00
slug: 2025-07-09-running-partdb-with-podman
type: posts
draft: false
categories:
  - software
tags: 
  - software
---


I recently reached a point where my electronics components inventory is becoming too huge to manually manage. My latest PCB project produced a BOM of about 50 parts, many of them, I already own. But which ones tho? Currently all of my SMD parts can fill a shoebox, searching through all of them would be a daunting task and could lead to parts missing from an order. To mitigate all of this I set up a [PartDB](https://docs.part-db.de/) instance in my laptop using Podman.

The process of running PartDB using Docker is very easy (all you have to do, is follow their [instructions](https://docs.part-db.de/installation/installation_docker.html)). Despite Podman being a very good drop-in replacement for Docker, I still ran into some issues that needed resolving.

# Changes
I've tested these changes on linux (Fedora 42)

Have a good look at the instructions [here](https://docs.part-db.de/installation/installation_docker.html).

You will need to install `podman` and `podman-compose`. You can think of podman-compose as a makefile for containers / a better way to handle all of the arguments needed when starting a container from the command-line.

In the suggested `docker-compose.yaml`, you will need to add a `:z` after each volume definition
```yaml {linenos=false style="github-dark"}
volumes:
  # By default
  - ./uploads:/var/www/html/uploads:z
  - ./public_media:/var/www/html/public/media:z
  - ./db:/var/www/html/var/db:z
```

With this change you should be able follow the instructions to complete the installation by replacing the docker name with podman.

The second issue appeared when the container was stopped and I needed to run `podman-compose up -d` to start it again. I was getting this error:
```sh {linenos=false style="github-dark"}
Error: creating container storage: the container name "partdb" is already in use by ******. You have to remove that container to be able to reuse that name: that name is already in use, or use --replace to instruct Podman to do so.
```
I couldn't get the `--replace` option to work, (As of July of 2025) like these guys [here](https://github.com/containers/podman-compose/issues/1118), so my only option is to remove the container before running it. I made a small makefile for this:

```makefile {linenos=false style="github-dark" }
all:
	podman rm -f partdb
	podman-compose up

remove:
	podman rm -f partdb

#init:
#	podman exec --user=www-data partdb php bin/console doctrine:migrations:migrate
```
I`ve commented out the database initialization command to not accidentally erase the database.

So far, PartDB has been working great for keeping track of my components. Only time can tell If I will be using it consistently to keep it up to date.
 







