#### MySQL的安装与运行
---

##### **tags:** #软件教程  
##### **2025-08-14**

---
## 安装包下载
-   [MySQL](https://www.oracle.com/cn/heatwave/free/)

## 安装步骤
1. 选择 custom，点击 next  
2. 点击 back，选择 full, 点击 next  
3. 再次点击 custom，点击 next  
   ***前三步作用为方便的选择需要安装的工具***
4. 单击选择的每个工具，选择合适的安装路径 (默认 C 盘，可跳过)  
5. 点击 next，点击 execute，后续默认选项略  
6. 输入并确认密码  
7. 在 Windows Service 界面，记住服务器名称 (大小写不敏感)，此处开机自启可选择取消，若遇到服务器名称占用问题，可使用管理员身份运行以下命令:  
```
sc delete MySQL80
```
8. 后续默认，略，最后一步为教程，可跳过

## 运行
- 开启关闭服务
```
//开启：使用管理员命令运行以下指令
net start MySQL80

//关闭：服务关闭
net stop MySQL80
```
- 打开 MySQL Workbench，connect