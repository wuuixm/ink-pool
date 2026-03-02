#### Wsl 2 中 archlinux 的配置与优化使用
---

##### **tags:**  #软件教程 
##### **2025-08-22**

---
## 教程同系统安装
- [[WSL2使用|在wsl2中安装archlinux]]

## 字体设置
- 点击 [MesloLGS NF](https://github.com/romkatv/powerlevel10k?tab=readme-ov-file#meslo-nerd-font-patched-for-powerlevel10k) 下载四个字体 (正常，粗体，斜体，粗斜体)
- 四个文件双击打开后安装
- 进入命令提示符的设置，打开 `setting.json` 将字体修改为 MesloLGS NF 字体 ![[code.png]]

## Git 配置
- 下载
```
panman -S git
```
- 更换代理，否则无法连接 github，只是使用主机名是为了防止 ip 变动，若为静态 ip，推荐使用主机 ip 代替主机名
```
// 主机中使用cmd获取主机名
hostname

git config --global https.proxy socks5://<主机名>:<端口号>

// 验证
git config --global --list
```

## Shell
**1. 将默认 shell 更换为 zsh**
- 下载 zsh
```
// 下载zsh
pacman -S zsh

// 验证下载
zsh --version
```
- 更换默认 shell
```
// 更换默认shell
chsh -s /bin/zsh

// 验证更换
echo $SHELL
```
**2. Oh-my-zsh 主题美化**
- 下载 oh-my-zsh
```
// 网络下载工具
pacman -S curl

// 下载oh-my-zsh
sh -c "$(curl -fsSL https://gitee.com/shmhlsy/oh-my-zsh-install.sh/raw/master/install.sh)"
```
- 查看内置主题可[点击预览](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)
```
// 在themes文件夹查看内置主题
cd ~/.oh-my-zsh/themes
```
- 内置主题设置
```
// 进入~文件夹下的配置文件
vim ~/.zshrc

// 找到主题对应配置项，修改
ZSH_THEME="要修改的主题名称"

// 刷新
source ~/.zshrc
```
- 经典主题 (`powerlevel10k`)
```
// 下载
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

// 配置
ZSH_THEME="powerlevel10k/powerlevel10k"

// 刷新
source ~/.zshrc

// p10k中的设置引导可随时跳出，再次启用使用
p10k configure
```
**3. Oh-my-zsh 辅助插件安装**
- 智能补全插件 `zsh-autosuggestions`
> [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) 是一个命令提示插件，当你输入命令时，会自动推测你可能需要输入的命令，按下**右方向键**可以快速采用建议
```
// 下载
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

// 打开配置文件
vim ~/.zshrc

// 填入插件名称
plugins=(
    # other plugins.
    zsh-autosuggestions
)

// 刷新
```
- 语法校验插件 `zsh-syntax-highlighting`
> [zsh-syntax-highlighting](https://link.zhihu.com/?target=https%3A//github.com/zsh-users/zsh-syntax-highlighting) 是一个命令语法校验插件，在输入命令的过程中，若指令不合法，则指令显示为红色，若指令合法就会显示为绿色
```
// 下载
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting 

// 配置同上
插件名称更换为 zsh-syntax-highlighting
```
- 快捷文件夹跳转插件 `z`
  Z 为内置插件，只需要将名称填入配置文件即可，无需下载
> z 是一个文件夹快捷跳转插件，对于曾经跳转过的目录，只需要输入最终目标文件夹名称，就可以快速跳转，避免再输入长串路径，提高切换文件夹的效率，使用时以 z 作为命令头即可