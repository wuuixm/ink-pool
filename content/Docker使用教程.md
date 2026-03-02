#### 以 arch 为例，使用 docker-compose 简化使用
---

##### **tags:**  #软件教程 
##### **2026-02-12**

---
## 下载
- 下载 docker 以及 docker-compose，两者配合，将基于命令的配置变为基于文件的配置
```
sudo pacman -S docker docker-compose
```

## 配置
- Docker 命令不做详解，下文只使用 `docker-compose`，基于 `yaml` 文件配置容器

## 使用 
- 开启 docker 服务：`sudo systemctl start docker.service`
- 在自定义路径创建任意名称文件夹，此文件夹作为 `docker-compose` 的配置，启动文件夹，文件夹名称可以是该容器的名称
- 在该文件夹下面创建名称为 `compose.yaml` 的文件，文件中可以配置版本，镜像，挂载卷的位置, 容器名称，环境变量等
- `cd` 到该文件夹下面，使用 `sudo docker-compose up -d` 首次启动容器
- 可以使用 `sudo docker ps` 命令会列出当前所有的正在运行的容器，`sudo docker ps -a` 命令会列出所有被创建的容器，包括被 `stop` 的
- 首次创建之后，之后可以使用 `sudo docker-compose up -d` 以及 `sudo docker-compose down` 命令来创建/销毁容器，也可以使用 `sudo docker-compose start/stop` 命令来开启/关闭一个已经创建的容器

## 示例
- 使用 `docker-compose` 来创建一个 `pgsql` 数据库容器
- 开启 docker 服务，创建 `pgsql-16` 文件夹，创建 `compose.yaml` 文件，写入以下内容，其中，`image` 代表镜像名称，`container_name` 代表容器名称，`restart` 代表重启时机，`environment` 代表环境变量，`ports` 代表向宿主机暴露的端口，前面为宿主机，后面为容器，`volumes` 代表使用挂载卷进行数据持久化，前面为当前文件夹的相对路径，后面为容器内的数据路径
```
services:
  postgres:
    image: postgres:16
    container_name: pgsql
    restart: unless-stopped

    environment:
      POSTGRES_USER: pgsql
      POSTGRES_PASSWORD: 123456 

    ports:
      - "127.0.0.1:5432:5432"

    volumes:
      - ./data:/var/lib/postgresql/data
```
- `cd` 到创建的文件夹，执行 `sudo docker-compose up -d` 命令，即可创建 pgsql 数据库容器，暴露于宿主机的 `5432` 端口

## 注意事项
- 可以将当前用于添加到用户组来避免频繁提权，命令为 `sudo usermod -aG docker $USER`，之后使用 `newgrp docker` 刷新
- 在没有特殊需要的情况下，推荐通过 `start/stop` 来使用容器，每次都 `up/down` 会反复擦写，另外，首次启动容器必须使用 `up -d` ，之后每次 `start` 无需携带 `-d` 参数，会自动后台运行
- Docker 有两种系统服务，分别为 `docker.service` 和 `docker.socket`，`docker.service` 是实际上的 docker 服务，可使用 `sudo systemctl enable --now docker.service` 命令来自启动，但是除非常态化使用 docker 或者用于服务器，不然不推荐自启动。`docker.socket` 是一个 `docker.service` 的钩子，当检测到使用到 docker 命令的时候，自动开启 docker 服务，可将 socket 配置为自启动：`sudo systemctl enable --now docker.socket`。个人使用更推荐第二种方式

