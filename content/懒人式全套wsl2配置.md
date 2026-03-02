---

---
#### 使用 wezterm，fishell，starship，fastfetch，yazi 工具配置 wsl 2 的 archlinux，相比[[Win终端与Arch配置|前笔记]]替换 nushell 为 fishell，增加 yazi 设置方便使用
---

##### **tags:** #笔记 
##### **2025-09-21**

---
## 下载
- 下载 `wezterm`
  [跳转下载](https://wezterm.org/index.html)
- 下载所需工具
```
pacman fish starship yazi fastfetch
```

## 配置
- `yazi` 配置参考笔记 [[Yazi]]
- `wezterm` 配置：在用户文件夹下创建 `.wezterm` 文件，可以自己配置，也可以替换为以下配置文件[云盘 wezterm 配置文件](https://wwxa.lanzouu.com/iqhNm36qle8j)，密码 `66am`，需要自己修改配置文件中的背景地址
- `starship` 配置，可在[官网](https://starship.rs/zh-CN/guide/)查看配色方案，推荐以下方案
```
starship preset pastel-powerline -o ~/.config/starship.toml
```
- `fastfetch` 配置，在 `~/.config/fastfetch/config.jsonc` 中设置，可以下载[云盘 fastfetch 配置文件](https://wwxa.lanzouu.com/i4XxO36rceqj)
- `fishell` 配置，配置文件位于 `~/.config/fish/config.fish`
```
// 将fish设为默认shell
chsh -s /usr/bin/fish

// 将starship，fastfetch以及yazi的快捷函数添加到fish配置中

if status is-interactive

    # Commands to run in interactive sessions can go here

  

    ###########初始化###########

    starship init fish | source

    fastfetch

    ###########################  

end

  

# yazi专有函数

function y

    set tmp (mktemp -t "yazi-cwd.XXXXXX")

    yazi $argv --cwd-file="$tmp"

    if read -z cwd < "$tmp"; and [ -n "$cwd" ]; and [ "$cwd" != "$PWD" ]

        builtin cd -- "$cwd"

    end

    rm -f -- "$tmp"

end
```