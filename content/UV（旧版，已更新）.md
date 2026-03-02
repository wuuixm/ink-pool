#### UV 的下载及常见使用
---

##### **tags:** #软件教程  
##### **2025-09-6**

---

## **安装包下载**
- [**UV下载地址**](https://github.com/astral-sh/uv)
- 以下以 windows 为例

## 安装步骤
- 下载对应的压缩包
- 解压到自定义文件夹
- 将该文件夹添加到环境变量 path 中
- 验证安装
```
uv --version
```

## 使用
- **初始化项目（必须）**
```
// 该命令会使用pyproject.toml用于管理依赖，而非传统re文件
uv init --python [版本]
```
- **下载指定版本 python 解释器**
  创建虚拟环境时不会明确下载指定版本，只会在本机寻找指定版本
```
uv python install [版本]
```
- **创建虚拟环境**
```
// uv venv [自定义保存目录，默认为当前cd目录] [python版本]
// 此处的命令行指定的python版本优先级小于init时文件写入的

uv venv D:\Envs\myenv --python 3.11
```
- **删除虚拟环境**
  无指令，直接删除 uv 所在文件夹即可
- **包下载指令**
```
//与pip区分，防止未激活环境时下载包导致全局污染  
uv pip install [package]  
//自动添加包到dependencies中，特性同上
uv add [package] [自定义分组，空为生产依赖，参数--dev为开发依赖]
```
- **包运行指令**
```
//uv安装的包不会自动注册全局，必须通过uv进行调用
uv run [包指令]
```
-  **包删除指令**
```
uv remove [package]
```
- **环境同步指令**
```
//自动下载pyproject.toml中的依赖包
uv sync
```
- **其余指令**
```
//列出uv支持的python 版本
uv python list

//下载对应版本的解释器
uv python install [python-version]

//指定python版本运行脚本
uv run -p [version] [py script]

//列出依赖树
uv tree  
```