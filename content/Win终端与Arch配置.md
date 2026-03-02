#### 使用 wezterm，nushell，starship，fastfetch 构建 WSL 2 的载体以及 Windows 终端，此处 WSL 发行版以 archlinux 为例，Windows 以 win 11 为例，[[Arch使用配置|本教程使用Nushell加starship代替了原本的zsh与omz]]，添加了 fastfetch
---

##### **tags:**  #软件教程 
##### **2025-08-22**

---
## 教程同系统安装
- [[WSL2使用|在wsl2中安装archlinux]]

## 软件下载
- **wezterm：** 跨平台终端软件，用于替代 wt，自定义程度极强，所有配置项可在[官网](https://wezterm.org/index.html)找到
- **nushell：** 全新 shell 软件，通过管道流通对象，而不是纯文本，[下载地址](https://github.com/nushell/nushell)
- **starship：** 命令提示符(`prompt`) 软件，用于 shell 的美化，支持自定义，下载流程、配置方法、以及预设方案可在[官网](https://starship.rs/zh-CN/guide/)查看
- **fastfetch：** `neofetch` 的替代软件，用于显示系统当前信息
- 上述三个软件下载
```
//wezterm，nushell，starship在win中通过官网下载，安装
//fastfetch在win中使用winget下载
winget install fastfetch

//三个个软件在arch中下载
pacman -S nushell starship fastfetch
```

## 构建流程
**1. Nushell 配置**
- Nushell 在 win 使用似乎有 bug，在 nushell 中使用 `echo $nu.config-path` 获取配置文件地址，向其中加入以下内容
```
# 配置 shell_integration，禁用 OSC133
$env.config = {
    shell_integration: {
        osc133: false
    }
}

// 刷新
source $nu.config-path
```
- 修改 nushell 为默认 shell
```
chsh -s /usr/sbin/nu
```
- 在 nushell 配置文件添加以下内容，用于禁止 nushell 启动输出信息
```
$env.config.show_banner = false
```
**2. Starship 配置**
- 接替 nushell 命令提示符，将以下内容添加到 nu 配置文件末尾
```
mkdir ($nu.data-dir | path join "vendor/autoload")
starship init nu | save -f ($nu.data-dir | path join "vendor/autoload/starship.nu")
```
- 下载预设，以 `Pastel Powerline` 主题为例
```
starship preset pastel-powerline -o ~/.config/starship.toml
```
**3. Wezterm 配置**
- 在用户文件夹创建 `.wezterm.lua` 文件用于配置终端，可以使用以下配置文件
```
-- 加载 wezterm API 和获取 config 对象
local wezterm = require 'wezterm'
local config = wezterm.config_builder()

config.launch_menu = {
  {
    label = 'PoserShell',
    args = { 'powershell.exe', '-NoLogo' },
  },
  {
    label = "Cmd",
    args = { 'cmd.exe'}
  }
}

-------------------- 颜色配置 --------------------
config.color_scheme = 'tokyonight_moon'
config.window_decorations = "RESIZE"
config.use_fancy_tab_bar = false
config.enable_tab_bar = true
config.show_tab_index_in_tab_bar = true
config.hide_tab_bar_if_only_one_tab = false
config.inactive_pane_hsb = {
  saturation = 0.9,
  brightness = 0.8,
}

-- 设置字体和窗口大小
config.font = wezterm.font("JetBrainsMonoNL NFM")
config.font_size = 12
config.initial_cols = 140
config.initial_rows = 30
config.default_prog = { 'nu' }

-------------------- 键盘绑定 --------------------
local act = wezterm.action
-- config.leader = { key = 'Alt', mods = 'NONE', timeout_milliseconds = 1000 }
config.leader = nil

-- 键盘绑定：使用 Alt 修饰键
config.keys = {
  { key = 'q',          mods = 'ALT',            action = act.QuitApplication },
  { key = 'l',           mods = 'SHIFT|CTRL',            action = act.SplitHorizontal { domain = 'CurrentPaneDomain' } },
  { key = 'j',          mods = 'SHIFT|CTRL',            action = act.SplitVertical { domain = 'CurrentPaneDomain' } },
  { key = 'q',          mods = 'SHIFT|CTRL',           action = act.CloseCurrentPane { confirm = false } },
  { key = 'h',  mods = 'ALT',     action = act.ActivatePaneDirection 'Left' },
  { key = 'l', mods = 'ALT',     action = act.ActivatePaneDirection 'Right' },
  { key = 'k',    mods = 'ALT',     action = act.ActivatePaneDirection 'Up' },
  { key = 'j',  mods = 'ALT',     action = act.ActivatePaneDirection 'Down' },

  -- CTRL + N 创建默认的Tab
  { key = 'n', mods = 'CTRL', action = act.SpawnTab 'DefaultDomain' },
  
  -- CTRL + M 关闭当前Tab
  { key = 'M', mods = 'CTRL', action = act.CloseCurrentTab { confirm = false } },

  -- CTRL + SHIFT + 1 创建新Tab - WSL
  {
    key = '*',
    mods = 'CTRL|SHIFT',
    action = act.SpawnCommandInNewTab {
      domain = 'DefaultDomain',
      args = {'wsl', '-d', 'Ubuntu-24.04'},
    }
  },
  {
    key = '&',
    mods = 'CTRL|SHIFT',
    action = act.SpawnCommandInNewTab {
      domain = 'DefaultDomain',
      args = {'wsl', '-d', 'archlinux'},
    }
  },
  {
    key = '@',
    mods = 'CTRL|SHIFT',
    action = act.SpawnCommandInNewTab {
      args = {'cmd'},
    }
  },
  {
    key = '#',
    mods = 'CTRL|SHIFT',
    action = act.SpawnCommandInNewTab {
      domain = 'DefaultDomain',
      args = {'pwsh'},
    }
  }
}

for i = 1, 8 do
  -- CTRL + number to activate that tab
  table.insert(config.keys, {
    key = tostring(i),
    mods = 'CTRL',
    action = act.ActivateTab(i - 1),
  })
end

-------------------- 鼠标绑定 --------------------
config.mouse_bindings = {
  -- copy the selection
  {
    event = { Up = { streak = 1, button = 'Left' } },
    mods = 'NONE',
    action = act.CompleteSelection 'ClipboardAndPrimarySelection',
  },
  
  -- Open HyperLink
  {
    event = { Up = { streak = 1, button = 'Left' } },
    mods = 'CTRL',
    action = act.OpenLinkAtMouseCursor,
  },
}

-------------------- 窗口居中 --------------------
wezterm.on("gui-startup", function(cmd)
        local screen = wezterm.gui.screens().active
        local width, height = screen.width * 0.5, screen.height * 0.5
        local tab, pane, window = wezterm.mux.spawn_window(cmd or {
                position = {
            x = (screen.width - width) / 2,
            y = (screen.height - height) / 2,
            origin = {Named=screen.name}
        }
        })
        window:gui_window():set_inner_size(width, height)
end)

--设置窗口透明度
config.window_background_opacity = 0.95
config.macos_window_background_blur = 10
-- config.background = {
--   {
--     source = {
--       File = 'D:/图片/拾光集/187.png',
--     },
--   }
-- }
return config
```
**4. Fastfetch 配置**
- 使用 `fastfetch --gen-config` 创建配置文件，位于 `~/.config/fastfetch/config.jsonc`
- 配置方案如下
```
{
    "$schema": "https://github.com/fastfetch-cli/fastfetch/raw/dev/doc/json_schema.json",
    "logo": {
        "type": "builtin",
        "source": "sulin",
        "padding": {
            "top": 1,
            "right": 9
        },
        "color": {
            "1": "green"
        }
    },
    "modules": [
        "title",
        "separator",
        {
            "type": "os",
            "keyColor": "magenta",
            "key": "OS"
        },
        {
            "type": "host",
            "keyColor": "magenta",
            "key": "Host" ,
            "format": "family"
        },
        {
            "type": "kernel",
            "keyColor": "magenta",
            "key": "Kernel"
        },
        {
            "type": "uptime",
            "keyColor": "magenta",
            "key": "Uptime"
        },
        {
            "type": "packages",
            "keyColor": "magenta",
            "key": "Packages"
        },
        {
            "type": "theme",
            "keyColor": "magenta",
            "key": "Theme"
        },
        
            "type": "font"
            "key": "Font"
            "keyColor": "magenta"
            "format": "{font1}
        }
        
            "type": "terminalfont"
            "keyColor": "magenta"
            "key": "Term Font
        }
        {
            "type": "cpu",
            "keyColor": "magenta",
            "key": "CPU"
        },
        {
            "type": "gpu",
            "key": "GPU",
            "keyColor": "magenta",
            "format": "{name}"
        },
        {
            "type": "memory",
            "key": "Memory",
            "keyColor": "magenta",
            "format": "{used} / {total} [{percentage}]"
        },
        {
            "type": "swap",
            "key": "Swap",
            "keyColor": "magenta",
            "format": "{used} / {total} [{percentage}]"
        },
        {
            "type": "disk",
            "key": "Disk",
            "keyColor": "magenta",
            "format": "{mountpoint} {size-used} / {size-total} [{size-percentage}]"
        },
        {
            "type": "localip",
            "key": "Local IP",
            "keyColor": "magenta",
            "compact": true
        }
        
            "type": "battery"
            "keyColor": "magenta"
            "key": "Battery
        }
        
            "type": "poweradapter"
            "keyColor": "magenta"
            "key": "Power" // 可能不受支持
        },
        "break",
        {
            "type": "colors",
            "paddingLeft": 12,
            "symbol": "circle"
        }
    ]
}
```
- 将 fastfetch 加入 nushell 配置文件，每次启动时自动运行
```
# 只在交互式 shell 下自动运行fastfetch
if $nu.is-interactive {
	// win中选择.exe
    fastfetch.exe
    
    // linux无后缀要求
    fastfetch
}
```