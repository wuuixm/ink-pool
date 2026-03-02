#### Yazi 是一个基于 tui 的文件管理软件，继承 vim 的是用习惯，主要用于类 unix 系统，windows 功能会有许多残缺，[[WSL2使用|本文以 wsl 2 为例演示]]
---

##### **tags:**  #软件教程 
##### **2025-09-21**

---
## 下载
- `yazi` 本体下载
```
pacman -S yazi
```
- `yazi` 官方文档
> https://yazi-rs.github.io/docs/installation/
- 所需工具下载
```
pacman -S vim mpv sxiv xdg-utils fzf
```

## 配置
- 创建配置文件
```
mkdir -p ~/.config/yazi

cd ~/.config/yazi

touch yazi.toml theme.toml keymap.toml
```
- 从文档复制默认配置文件到对应文件中
- 修改 `yazi.toml` 为[云盘 yazi 文件](https://wwxa.lanzouu.com/i9gRA36oflkf)
- 修改 `keymap.toml` 为[云盘 keymap 文件](https://wwxa.lanzouu.com/iBMHN36ofllg)
  
## 常用指令
- 单键操作
```
a ==> 创建文件或文件夹，末尾加上/即为文件夹
r ==> 重命名选中的文件
x ==> 剪切选中的文件
y ==> 复制文件
p ==> 粘贴
d ==> 删除选中的文件到回收站
D ==> 直接删除选中的文件
o ==> 打开选中的文件
O ==> 可以选择使用不同的方式打开文件
h ==> 返回上级目录
j ==> 光标向下移动
k ==> 光标向上移动
l ==> 进入选中的目录
space ==> 选中
z ==> 调用fzf全局搜索
/ ==> 高亮所选搜索文件
n ==> 切换到下一个搜索项
N ==> 切换到上一个搜索项
t ==> 创建新标签页
q ==> 退出yazi
```
- 快捷操作
```
gg ==> 跳到列表顶部
G ==> 跳到解表底部
? ==> 显示帮助
f ==> 筛选器，不使用高亮，只显示搜索的文件
: ==> 执行 shell 命令
```

## 补充
- 下完完成之后可以通过 `yazi` 来打开界面
- `yazi` 默认没有退出之后将打开之后的路径直接加载到终端的功能，可以通过在 `shell` 中写入函数实现，写入函数之后可以直接使用 `y` 启动 yazi， 以 `fish` 为例，其余 shell 的写法在 `yazi` 文档中查看
```
// 打开~/.config/fish/config.fish，写入以下函数

function y
	set tmp (mktemp -t "yazi-cwd.XXXXXX")
	yazi $argv --cwd-file="$tmp"
	if read -z cwd < "$tmp"; and [ -n "$cwd" ]; and [ "$cwd" != "$PWD" ]
		builtin cd -- "$cwd"
	end
	rm -f -- "$tmp"
end
```
