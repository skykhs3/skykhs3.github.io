---
title: "Frequently Used Docker Commands"
date: 2025-06-15 18:00:00 +09:00
categories: [Tools,  Docker]
author: skykhs3
image:
  path: /assets/img/posts/2025-06-15-frequently-used-docker-commands/docker-logo.svg
  alt: Docker
  show_in_post: false
tags:
  - docker
description: A collection of frequently used Docker commands
---

<details>
<summary>
<span markdown="1">
`docker version`
</span>
</summary>
<div markdown="1">
```bash
Client:
 Version:           27.5.1-rd
 API version:       1.47
 Go version:        go1.22.11
 Git commit:        0c97515
 Built:             Thu Jan 23 18:12:38 2025
 OS/Arch:           darwin/arm64
 Context:           desktop-linux

Server: Docker Desktop 4.42.0 (195023)
 Engine:
  Version:          28.2.2
  API version:      1.50 (minimum version 1.24)
  Go version:       go1.24.3
  Git commit:       45873be
  Built:            Fri May 30 12:07:27 2025
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.7.27
  GitCommit:        05044ec0a9a75232cad458027ca83437aae3f4da
 runc:
  Version:          1.2.5
  GitCommit:        v1.2.5-0-g59923ef
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```
</div>
</details>

## 1. Image & Container Management

| Command | Description |
|---------|-------------|
| [`docker pull <image-name>`](https://docs.docker.com/reference/cli/docker/image/pull/) | Pull image from registry |
| [`docker create --name <container-name> <image-name>`](https://docs.docker.com/reference/cli/docker/container/create/) | Create container without running it |
| [`docker run -it --name <container-name> <image-name>`](https://docs.docker.com/reference/cli/docker/container/run/) | Create and Run container interactively |
| [`docker exec -it <container-name> <command>`](https://docs.docker.com/reference/cli/docker/container/exec/) | Execute command in running container |
| [`docker ps -a`](https://docs.docker.com/reference/cli/docker/container/ps/) | List all containers |
| [`docker start <container-name>`](https://docs.docker.com/reference/cli/docker/container/start/) | Start container |
| [`docker stop <container-name>`](https://docs.docker.com/reference/cli/docker/container/stop/) | Stop container |
| [`docker logs <container-name>`](https://docs.docker.com/reference/cli/docker/container/logs/) | Show logs |
| [`docker rm <container-name>`](https://docs.docker.com/reference/cli/docker/container/rm/) | Remove container |
| [`docker rmi <image-name>`](https://docs.docker.com/reference/cli/docker/image/rm/) | Remove image |

## 2. Volume Management

| Command | Description |
|---------|-------------|
| [`docker volume ls`](https://docs.docker.com/reference/cli/docker/volume/ls/) | List volumes |
| [`docker volume create <volume-name>`](https://docs.docker.com/reference/cli/docker/volume/create/) | Create volume |
| [`docker volume rm <volume-name>`](https://docs.docker.com/reference/cli/docker/volume/rm/) | Remove volume |
| [`docker run -v <volume-name>:<container-path> <image-name>`](https://docs.docker.com/reference/cli/docker/container/run/) | Mount volume to container |
| [`docker run -v <host-path>:<container-path> <image-name>`](https://docs.docker.com/reference/cli/docker/container/run/) | Mount host path to container |


> Press `Ctrl + p + q` to exit the terminal without stopping the container
{: .prompt-tip }

## 3. System Management

| Command | Description |
|---------|-------------|
| [`docker system df -v`](https://docs.docker.com/reference/cli/docker/system/df/) | Show Docker daemon disk usage |
| [`docker system prune`](https://docs.docker.com/reference/cli/docker/system/prune/) | Remove unused data |

## 4. Docker Prune Commands Comparison

| Command | Stopped<br>Containers | Unused<br>Networks | Dangling<br>Images | Unused<br>Images | Unused<br>Anonymous<br>Volumes | Unused<br>Named<br>Volumes |
|---------|----------------------|-------------------|-------------------|---------------------|---------------------------|------------------------|
| `docker system \`<br>`prune` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| `docker system \`<br>`prune --volumes` | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ |
| `docker system \`<br>`prune -a` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| `docker system \`<br>`prune -a --volumes` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| `docker volume \`<br>`prune` | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| `docker volume \`<br>`prune -a` | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |

## 5. Examples

```bash
# Pull Ubuntu image
docker image pull ubuntu:24.04
```
```bash
# Run Ubuntu container
docker container run -it -v ./test-volume:/home/ubuntu --name ubuntu-test ubuntu:24.04
```
```bash
# Execute bash in container
docker container exec -it ubuntu-test /bin/bash
```

## 6. Docker Compose Commands

| Command | Description |
|---------|-------------|
| [`docker compose up`](https://docs.docker.com/reference/cli/docker/compose/up/) | Apply changed `compose.yml` and start services |
| [`docker compose up <service-name>`](https://docs.docker.com/reference/cli/docker/compose/up/) | Start a specific service |
| [`docker compose down`](https://docs.docker.com/reference/cli/docker/compose/down/) | Stop services |
| [`docker compose down -v`](https://docs.docker.com/reference/cli/docker/compose/down/) | Stop services and remove volumes |
| [`docker compose ls`](https://docs.docker.com/reference/cli/docker/compose/ls/) | List all projects |

> `docker compose up --build` rebuilds the images
{: .prompt-tip }