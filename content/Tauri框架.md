---

---
#### 类 electron 框架，使用 rust 代替后端的 js 部分，取消 chromium 内置，使用 webview 替代
---

##### **tags:** #笔记 
##### **2025-10-17**

---
## 基础环境
- 搭建 rust 环境，参考 [[Rust环境搭建（Windows）]]
- 搭建 TS/JS 环境，自行解决


## 创建
- 使用 `cargo` 下载 `tauri简易创建工具`
```
cargo install create-tauri-app
``` 
- 检查 `cargo` 工具包
```
cargo install --list
```
- 执行以下命令，在当前文件夹下创建 `tauri` 模板
```
cargo-create-tauri-app

// 若项目位置一项选择<.>，可直接将当前文件夹初始化为tauri项目
```
