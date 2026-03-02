

## 1. 文件和目录操作

- **`ls`**：列出目录内容
    
    `ls -l      # 长列表显示 ls -a      # 显示隐藏文件`
    
- **`cd`**：切换目录
    
    `cd /home/user cd ..      # 上一级目录`
    
- **`pwd`**：显示当前路径
    
    `pwd`
    
- **`mkdir` / `rmdir`**：创建/删除目录
    
    `mkdir test_dir rmdir test_dir`
    
- **`rm -r`**：删除目录及内容
    
    `rm -r test_dir`
    
- **`cp` / `mv`**：复制 / 移动（重命名）文件
    
    `cp file1.txt file2.txt mv file.txt newfile.txt`
    
- **`touch`**：创建空文件
    
    `touch newfile.txt`
    
- **`cat` / `less` / `more`**：查看文件内容
    
    `cat file.txt less file.txt more file.txt`
    
- **`head` / `tail`**：查看开头/结尾行
    
    `head -n 10 file.txt tail -n 20 file.txt`
    

---

## 2. 权限与用户管理

- **`chmod`**：修改文件权限
    
    `chmod 755 script.sh`
    
- **`chown`**：修改文件属主
    
    `chown user:group file.txt`
    
- **`passwd`**：修改密码
    
    `passwd`
    
- **`whoami` / `id`**：查看当前用户信息
    
    `whoami id`
    
- **`useradd` / `usermod` / `userdel`**：添加/修改/删除用户
    
    `useradd -m newuser passwd newuser usermod -aG wheel newuser userdel -r olduser`
    
- **`sudo`**：以 root 权限执行命令（需安装）
    
    `sudo pacman -S vim`
    

---

## 3. 系统管理

- **`top` / `htop`**：查看进程/资源
    
    `top htop      # 如果已安装`
    
- **`ps`**：查看进程
    
    `ps aux`
    
- **`kill` / `killall`**：结束进程
    
    `kill 1234       # PID killall firefox`
    
- **`df` / `du`**：查看磁盘使用情况
    
    `df -h du -sh /home/user`
    
- **`free`**：查看内存
    
    `free -h`
    
- **`uname -a`**：系统信息
    
    `uname -a`
    
- **`hostname`**：查看/设置主机名
    
    `hostname hostname NewHostName`
    
- **`reboot` / `shutdown`**：重启/关机（WSL 无需）
    

---

## 4. 软件安装与管理

- **`pacman`（Arch）**
    
    `sudo pacman -S package_name    # 安装软件 sudo pacman -R package_name    # 卸载软件 sudo pacman -Syu               # 更新系统`
    
- **`apt`（Ubuntu/Debian）**
    
    `sudo apt update sudo apt install package_name sudo apt remove package_name`
    
- **`curl` / `wget`**：下载文件
    
    `curl -O https://example.com/file.sh wget https://example.com/file.sh`
    
- **`git clone`**：克隆远程仓库
    
    `git clone https://github.com/user/repo.git`
    

---

## 5. 文本处理

- **`grep`**：搜索文本
    
    `grep "keyword" file.txt grep -r "keyword" ./folder`
    
- **`sed`**：流编辑
    
    `sed 's/old/new/g' file.txt`
    
- **`awk`**：字段处理
    
    `awk '{print $1,$3}' file.txt`
    
- **`cut`**：提取字段
    
    `cut -d',' -f1 file.csv`
    
- **`sort` / `uniq` / `wc`**：排序、去重、统计
    
    `sort file.txt uniq file.txt wc -l file.txt`
    

---

## 6. 压缩与打包

- **`tar`**
    
    `tar -xvf archive.tar       # 解压 tar -czvf archive.tar.gz folder/   # 压缩`
    
- **`zip` / `unzip`**
    
    `zip -r archive.zip folder/ unzip archive.zip`
    
- **`gzip` / `gunzip`**
    
    `gzip file.txt gunzip file.txt.gz`
    

---

## 7. 网络与远程

- **`ping`**：测试网络
    
    `ping google.com`
    
- **`ssh` / `scp`**：远程登录/文件拷贝
    
    `ssh user@host scp file.txt user@host:/path/`
    
- **`curl` / `wget`**：HTTP/FTP 下载
    
    `curl -O https://example.com/file wget https://example.com/file`
    

---

## 8. 进阶与 shell 配置

- **`alias`**：命令别名
    
    `alias ll='ls -alF'`
    
- **`export`**：环境变量
    
    `export PATH=$PATH:/usr/local/bin`
    
- **`source` / `.`**：加载配置文件
    
    `source ~/.bashrc . ~/.zshrc`
    
- **`chsh`**：修改默认 shell
    
    `chsh -s $(which zsh)`
    
- **`history`**：查看命令历史
    
    `history`
    
- **`man`**：查看命令手册
    
    `man ls`
---

## 9. **多任务与多会话管理**

- `jobs`：查看当前 shell 的作业列表（前台/后台任务）
    
    `jobs`
    
- `fg %作业号`：将后台任务搬到前台
    
    `fg %1`
    
- `bg %作业号`：将暂停的任务放到后台继续运行
    
    `bg %1`
    
- `&`：在后台启动命令
    
    `long_command &`
    
- `nohup`：忽略终端关闭信号，让任务在后台持续运行
    
    `nohup long_command &`
    
- `tmux`：终端复用器，在一个物理终端里创建多个虚拟会话/窗口
    
    `tmux new -s mysession      # 新建会话 Ctrl+B C                    # 新建窗口 Ctrl+B 0/1/2                # 切换窗口 Ctrl+B D                    # detach 会话 tmux attach -t mysession    # 重新 attach 会话`
