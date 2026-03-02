#### 基于配置文件方式使用 distrobox，使用非共享家目录方式实现
---

##### **tags:**  #软件教程 
##### **2026-02-15**

---
## 下载
```
sudo pacman -S distrobox
```

## 配置
- 需要 `docker` 或者 `podman` 作为[[Docker使用教程|前置]]
- 需要将自身加入 docker 用户组，``sudo usermod -aG docker $USER``
- 需要启动 `docker`

## 使用
- 创建文件夹
- 编写 `distrobox.ini` 文件
- 使用 `distrobox assemble cteate --file ./distrobox.ini` 命令创建容器
- 使用 `distrobox enter 容器名称` 来进入容器
- 容器无需手动关闭

## 示例
- 使用 `distrobox` 来创建 `ubuntu-22.04` 环境
- 文件配置如下
```
// 容器名称
[ubuntu-2204] 

// 使用的基础服务
manager=docker 

// 镜像
image=ubuntu:22.04 

// 重建是否拉取最新
pull=true 

// 初始化勾子，根据个人需要来创建
init_hooks=export _JAVA_AWT_WM_NONREPARENTING=1; export QT_QPA_PLATFORM=xcb;export GDK_BACKEND=x11;ln -sfn /src/Workspace ~/Workspace

// 附加包
additional_packages="sudo git curl neovim fish ca-certificates"

// 容器内家目录
home=/home/wuuixm/Containers/distrobox/ubuntu-2204

// 挂载的文件树
volume="/home/wuuixm/Workspace:/src/Workspace"
volume="/home/wuuixm/Assets/dotfiles:/src/dotfiles"

```

## 注意事项
- 本文使用了非共享家目录的方式，这种方式可以保持宿主机家目录的纯净，但是当写跨系统脚本的时候，路径指定非常麻烦，需要多次尝试
- 对于 `kde` 等正常 de 来说，`distrobox-export -app 可执行文件路径` 可以正常导出 `.desktop` 文件，但是如果使用 `niri` 等 wm 软件，不要使用自动导出方式，需要手动创建 `.desktop` 文件，其中，当需要使用指定环境变量启动软件的时候，一定要用从宿主机到容器的绝对路径