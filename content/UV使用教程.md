#### Python 版本以及包管理工具 uv 的使用教程，以 linux 为例，windows 除 uv 下载方式以外，无使用区别
---

##### **tags:**  #软件教程 
##### **2026-03-02**

---
## 下载
- 使用 `sudo pacman -S uv` 下载

## 配置
- 使用 `uv tree` 或者 `uv python list` 列出所有可安装以及已经安装的解释器
- 使用 `uv pip list` 列出当前项目安装的包
- 使用 `uv python install --python 版本号` 下载需要的 python 解释器
- 使用 `uv python uninstall --python 版本号` 删除指定版本的解释器
- 使用 `uv init 路径 --python 版本号` 创建项目，以及指定写入项目配置文件的最低解释器版本号
- 使用 `uv venv --python 版本号` 创建指定版本的虚拟环境
- 使用 `uv add` 命令添加包
- 使用 `uv remove` 删除包

## 示例
- 使用 `uv python install --python 3.12` 安装解释器
- 使用 `uv init ~/uuu --python 3.12` 在家目录下面创建 `uuu` 项目，指定版本最低为 3.12
- 跳转到项目目录，使用 `uv venv --python 3.12` 创建与项目版本相对应的虚拟环境
- 使用 `uv add requests` 安装包

## 提示
- 在初始化项目之后，可以跳过手动创建虚拟环境这一步，直接使用 `uv add` 命令，会自动创建与项目的版本对应的虚拟环境
- 可以在 `uv add` 命令后添加 `--index` 来使用指定下载源，覆盖默认下载源
- 可以在 `uv add` 命令后添加 `--extra-index` 来附加指定下载源
- 可以通过声明 `pyprojext.toml` 的方式来指定某些包的下载方式，以下为一个单独指定 `pytorch` 下载源，不影响其他包的配置
```
[[tool.uv.index]]
name = "pytorch-cu121"
url = "https://download.pytorch.org/whl/cu121"
explicit = true

[tool.uv.sources]
torch = { index = "pytorch-cu121" }
torchvision = { index = "pytorch-cu121" }
torchaudio = { index = "pytorch-cu121" }
```