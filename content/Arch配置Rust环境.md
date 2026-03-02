#### 在 wsl 2 archlinux 中配置 rust 全套工具链，并且从主机调用 vsc 编写 rust 代码
---

##### **tags:**  #软件教程 
##### **2025-08-21**

---
## 安装
- [[WSL2使用|在 wsl 2 中安装 archlinux]]

## 必备工具
**1. 安装 rustup**
```
pacman -S rustup
```
**2. 安装 rust 工具链**
```
rustup default stable
```
- 此工具链会安装部分组件，包括编译器 `rustc`，包管理器 `cargo` ，标准库 `std`，离线文档 `rust-docs`，格式化工具 `rustfmt` 和代码检查 `clippy`
- 编译器，包管理器与标准库是必备工具，其余工具如无需要可以删除
```
rustup component remove rust-docs
```
**3. 安装 lsp 组件**
```
rustup component add analyzer
```
**4. 安装 C 编译工具**
```
pacman -S base-devel
```
- `base-devel` 是为arch 准备的 C 语言工具包，包含 `gcc`，这里安装 `base-devel` 而不是 `gcc` 是为了防止出现缺少工具

## Vsc 所需的插件
**1. 已经有 Rust 配置文件**
- 如果已经有 Rust 的配置文件了，就可以直接使用 remote 插件连接 wsl，缺失的插件会提示下载，无需后续教程
**2. 无 Rust 配置文件**
- 必备插件：`rust-analyzer` lsp 工具，提供语言服务
- 辅助插件：
  - `Cargo` 集成包管理插件，提供可视化 run 和 debug
  - `CodeLLDB` 提供 Rust 调试功能，支持断点，查看等功能
  - `Dependi` 显示 cargo 的 toml 文件中的依赖版本
  - `Even Better TOML` toml 语法高亮插件