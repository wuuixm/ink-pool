#### SQL Server 2022 的安装与初次使用
---

##### **tags:** #软件教程  
##### **2025-08-14**

---
## 安装包下载
- [SQL Server 2022 安装包](https://www.microsoft.com/zh-cn/sql-server/sql-server-downloads)
- [SSMS](https://learn.microsoft.com/zh-cn/ssms/download-sql-server-management-studio-ssms)

## 安装步骤
- ***安装前关闭防火墙***
- **SQL server 安装**
1. 下载安装包
2. 选择自定义安装
3. 选择全新安装
4. 功能模块选择引擎与复制服务
5. 如出现 Azure 界面，取消勾选
6. 服务设置为自动 (手动也可以，根据需求选择)
7. 身份验证选择混合模式
8. 点击添加当前用户
9. 点击安装
- **SSMS**
1. 安装
2. 重启

## 运行
1. 打开 SQL Server 配置管理器
2. 启动所有服务
3. 点击网络配置，启用 TCP/IP
4. 重启 SQL Server 服务
5. 打开 SSMS，连接到本地服务器