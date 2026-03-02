#### 使用 cli 形式 git 完成常用操作 
---

##### **tags:**  #软件教程 
##### **2025-09-15**

---
## 安装包下载
[点击跳转下载界面](https://git-scm.com/downloads/win)

## 安装步骤
- 安装步骤非常多，具体下载选项参考下文
  > https://blog.csdn.net/mukes/article/details/115693833
  
## 配置
- 设置 git 代理
```
// 设置http，https的代理，具体端口根据自己的机器设置
git config --global http.proxy http://127.0.0.1:7897
git config --global https.proxy https://127.0.0.1:7897
```
- 开启凭证助手，需要 **特别注意**，github 目前的精细度令牌没有合适的凭证管理器支撑，助手仍然只会使用上次缓存的令牌匹配所有请求，推荐仍然使用经典令牌
```
// 使用gcm作为凭证助手
git config --global credentials.helper manager-core
// 若是没有gcm也可以使用store作为凭证助手，但是令牌会明文存于~/.git-credentials文件
git config --global credentials.helper store
```

## 初始化
**1. 使用 github 初始化**
- 此种方式最简单，稳定，常用，避免双端分支不同问题，且经典令牌和精细令牌都可使用，为最推荐方式
- 在 github 中创建仓库
- 直接克隆到本地
```
// 在对应文件夹打开或是cd到对应文件夹，打开git bash
git clone https://github.com/[用户名]/[仓库名].git
```
**2. 从本地初始化仓库**
- 此种方式复杂且容易出现问题，还需要处理跟踪关系，分支名称不同问题，只推荐使用 `github desktop` 时使用此种方式
- 创建仓库
```
//仍然需要在github上创建仓库
// 选择一个文件夹作为本地仓库，打开git bash
```
- 首次提交
```
// 初始化仓库
git init

// 未连接仓库时可以执行暂存与提交
echo "create a submission" > README.md
git add .
git commit -m "this is the first submission"
```
- 连接仓库
```
// 连接到远程仓库
git remote add origin https://github.com/[用户名]/[仓库名].git

// 首次跟踪，当前github使用main作为默认分支，旧版git默认创建的是master本地分支，使用以下命令处理
git push -u origin master:main

//新版git可以不需要处理，直接首次跟踪
git push -u origin main
```

## 常用操作
**1. 基础指令**
```
// 暂存指令
git add [需要暂存的文件，使用.可以匹配所有修改]

// 查看指令
git status

// 提交指令
git commit -m "[改次提交的注释]"

// 拉取指令
git pull

// 推送指令
git push origin master:main

// 创建新分支
git branch [分支名称]
```
**2. 常用指令**
```
// 查看提交历史
git log

// 查看某个文件的提交历史
git log --filename

// 查看所有分支
git branch -a

// 切换分支
git switch [分支名称]

// 合并分支，需要先切换到需要合并的分支
git merge [分支名称]

// 删除分支
git branch -d [分支名称]   # 删除本地分支
git push origin --delete [分支名称]   # 删除远程分支
```
**3. 回溯项目**
- 仅查看
```
// 使用git log查看提交历史的哈希值

// 回溯到指定状态
git checkout [哈希值]

// 回到分支
git checkout [分支名称]
```
- 从指定节点创建分支
```
// 获取哈希值

// 回溯并创建分支
git checkout -b [新分支] [哈希值]
```
- 完全回溯
```
// 获取哈希值

// 重置项目到指定节点
git reset --hard [哈希值]
```
