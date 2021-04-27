# The Docker Handbook

*Notes and examples from [The Docker Handbook](https://www.freecodecamp.org/news/the-docker-handbook/) by [Farhan Chowdhury](https://twitter.com/frhnhsin)*

## Introduction

### Container Runtime

The container runtime (Docker) sites between the containers and the host operating system.  As a demonstration, enter the following into a terminal:

```bash
uname -a
# Linux HOSTNAME 5.4.72-microsoft-standard-WSL2 #1 SMP Wed Oct 28 23:40:43 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux

docker run alpine uname -a
# Unable to find image 'alpine:latest' locally
# latest: Pulling from library/alpine
# 540db60ca938: Pull complete
# Digest: sha256:69e70a79f2d41ab5d637de98c1e0b055206ba40a8145e7bddb55ccc04e13cf8f
# Status: Downloaded newer image for alpine:latest
# Linux 7cf4cf5e1253 5.4.72-microsoft-standard-WSL2 #1 SMP Wed Oct 28 23:40:43 UTC 2020 x86_64 Linux
```

This shows [Ubuntu](https://ubuntu.com/blog/ubuntu-on-wsl-2-is-generally-available) (in my case) and [Alpine Linux](https://alpinelinux.org/) both using the WSL2 kernel.

### Images

Images are multi-layered, self-contained files that act as the template for creating containers. Containers are just images in a running state.

### Registry

A centralised image registry to upload and download images.

### Architecture

> "Docker uses a client-server architecture. The Docker *client* talks to the Docker *daemon*, which does the heavy lifting of building, running, and distributing your Docker containers" [*Docker documentation*](https://docs.docker.com/get-started/overview/#docker-architecture)

## Usage

### Detached Mode

To run a container as a background process, simply use the `--detach` or `-d` option.

```bash
docker container run --detach --publish 8080:80 fhsinchy/hello-dock
```

### List Containers

List running containers with:

```bash
# currently running
docker container ls

# include containers that have run in the past
docker container ls -all # -a
```



## Examples

### Hello World

With Docker installed, open a terminal and run the following:

```bash
docker run hello-world

# Unable to find image 'hello-world:latest' locally
# latest: Pulling from library/hello-world
# b8dfde127a29: Pull complete
# Digest: sha256:f2266cbfc127c960fd30e76b7c792dc23b588c0db76233517e1891a4e357d519
# Status: Downloaded newer image for hello-world:latest

# Hello from Docker!
# This message shows that your installation appears to be working correctly.

# To generate this message, Docker took the following steps:
#  1. The Docker client contacted the Docker daemon.
#  2. The Docker daemon pulled the "hello-world" image from the Docker Hub. (amd64)
#  3. The Docker daemon created a new container from that image which runs the executable that produces the output you are currently reading.
#  4. The Docker daemon streamed that output to the Docker client, which sent it to your terminal.

# To try something more ambitious, you can run an Ubuntu container with:
#  $ docker run -it ubuntu bash

# Share images, automate workflows, and more with a free Docker ID:
#  https://hub.docker.com/

# For more examples and ideas, visit:
#  https://docs.docker.com/get-started/
```

The [Hello World!](https://hub.docker.com/_/hello-world) image is an example of minimal containerization with Docker.

Run `docker ps -a` in a terminal to see current and past containers that have run on the system:

```bash
docker ps -a
# CONTAINER ID   IMAGE                              COMMAND                  CREATED         STATUS                     PORTS                                            NAMES
# 1abb22b58e7d   hello-world                        "/hello"                 5 minutes ago   Exited (0) 5 minutes ago                   
```

### Hello Dock

This image contains a simple Vue.js application running on port 80 (container) which is published as 8080 (host).

```bash
docker container run --publish 8080:80 fhsinchy/hello-dock

# Unable to find image 'fhsinchy/hello-dock:latest' locally
# latest: Pulling from fhsinchy/hello-dock
# 0a6724ff3fcd: Pull complete
# ...
# /docker-entrypoint.sh: Configuration complete; ready for start up
```

To allow access into a container, you must map host and container ports.  The example above forwards any requests to 8080 on the host system to 80 inside the container.

To access the application, visit `http://127.0.0.1:8080/` in a browser.