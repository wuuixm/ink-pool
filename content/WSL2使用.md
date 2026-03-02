#### Wsl 使用全流程，以安装 archlinux 为例，默认已开启 wsl 2
---

##### **tags:**  #软件教程 
##### **2025-08-21**

---
## 下载
- 使用 wsl 命令列出支持的发行版
```
wsl --list --online
``` 
- 下载需要的版本，以 archlinux 为例
```
wsl --install -d Archlinux --location <指定安装位置>
```

## 配置
- Arch 默认无 sudo 工具，初次使用即为 root 用户
- 其他发行版，如 ubuntu 初次使用需要配置用户