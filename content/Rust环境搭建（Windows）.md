---

---
#### 在 windows 中搭建 rust 环境
---

##### **tags:** #笔记 
##### **2025-10-17**

---
## Win 特有配置
- 安装 `windows sdk`
  下载 `virual studio`，选择 `c++` 的桌面开发环境，在其中勾选 `windows sdk`，该工具用于 `rustc` 编译程序

## 环境配置流程

**1. 从 rustup 启动 rust 组件安装程序**，下面的三种安装方式都可以，`winget` 方式会快一些，`scoop` 方式更加便捷
- 从 [rustup 网站](https://rustup.rs/)下载 `rustup-init.exe`，双击之后默认回车，自动下载稳定版本
- 使用 `winget` 方式下载
```
winget install Rustlang.Rustup
```
- 使用 `scoop` 方式下载
```
scoop install rust
```
- 以上两种包管理器下载方式一般下载完之后会自动执行 `rustup-init`，下载 rust 依赖组件，若未开始，使用以下命令下载稳定版
```
rustup default stable
```

**2. 检查核心组件**
- `cargo`：rust 包管理器与构建器，用于管理所有 rust 项目的项目依赖以及全局工具
- `rustc`：rust 编译器
- `rust-std`：rust 标准库
- `rust-analyzer`：rust 语言服务器，用于为 ide 提供代码高级功能
- `clippy`：代码分析工具，检查错误
- `rustfmt`：格式化工具
- **以最核心组件为例，检查安装是否完成**
```
rustc --version
cargo --version
```


## 使用
罗列 rust 常用指令，几乎所有使用都是以 cargo 工具为中心
- `cargo new <name>`：在当前文件夹下创建 `<name>` 项目
- `cargo init`：初始化当前文件夹为 rust 项目

- `cargo add <crate>`：添加项目依赖，自动记录到配置文件
- `cargo add <crate>@<version>`：添加指定版本的项目依赖
- `cargo add <crate> --dev`：添加项目依赖为开发依赖
- `cargo add <crate> --build`：添加依赖为构建依赖
- `cargo remove <crate>`：移除指定依赖

- `cargo build`：以 `dev` 模式编译项目，用于快速编译
- `cargo build --release`：以 `release` 模式编译项目，优化更多，编译更慢，用于最终打包
- `cargo run`：编译并运行
- `cargo run --release`：编译并运行 `release` 版本
- `cargo check`：检查代码是否能通过编译

- `cargo install`：安装全局工具
- `cargo install --version`：安装指定版本全局工具
- `cargo uninstall`：删除全局工具

- `cargo clean`：清除构建产物以及二进制文件
- `cargo clean --release`：清除 `release` 构建产物