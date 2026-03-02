#### 自组下载器前后端，解决使用gui 下载器资源消耗过多的问题，环境为 archlinux，zen browser
---

##### **tags:**  #软件教程 
##### **2026-02-23**

---
## 下载
- 使用 `sudo pacman -S aria2` 下载 `aria2`
- 下载浏览器插件，`firefox` 商店中搜索 `Aria2 Integration`

## 配置
- 创建 `aria2` 文件
```
mkdir -p ~/.config/aria2
touch ~/.config/aria2/aria2.conf
touch ~/.config/aria2/aria2.session
```
- 将以下配置文件写入 `aria2.conf`，路径自行修改
```
# 下载目录
dir=/home/wuuixm/Downloads

# 启用 RPC
enable-rpc=true
rpc-listen-all=false
rpc-allow-origin-all=true
rpc-listen-port=6800

# 多线程优化
max-connection-per-server=8
split=8
min-split-size=10M

# 断点续传
continue=true

# 会话保存
input-file=/home/wuuixm/.config/aria2/aria2.session
save-session=/home/wuuixm/.config/aria2/aria2.session
save-session-interval=60

# 降低磁盘碎片
file-allocation=trunc

# 伪造UA
user-agent=Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0 Safari/537.36
header=Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
header=Accept-Language: en-US,en;q=0.5
header=Connection: keep-alive
```
- 使用 `systemd` 创建用户服务，方便开启服务已经自启动
```
mkdir -p ~/.config/systemd/user/
touch ~/.config/systemd/user/aria2.service
```
- 将以下文件写入 `aria2.service` 文件
```
[Unit]
Description=Aria2 Download Manager

[Service]
ExecStart=/usr/bin/aria2c --conf-path=/home/wuuixm/.config/aria2/aria2.conf
Restart=on-failure

[Install]
WantedBy=default.target
```
- 使用 `systemctl --user enable --now aria2` 开启 `aria2` 服务用户级自启动
- 在 `Aria2 Integration` 浏览器插件中添加 `Localhost` 本地服务器，端口一般为 `6800` ，最后勾选开启拦截下载
