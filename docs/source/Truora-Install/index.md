# 安装部署

Truora-Service本身是个典型的SpringBoot工程，只要熟悉Java以及相关的知识，则可以按照一般Java项目的开发部署模式，完成全流程的操作。

当前版本聚焦于代码级的开发编译部署。

## 1：从github/gitee获取Truora-Service代码
```
git clone https://github.com/WeBankBlockchain/Truora-Service.git
git clone https://gitee.com/WeBankBlockchain/Truora-Service.git
```

## 2: 构建开发环境
结合自己的开发环境IDE导入Java项目

## 3：开发编译
参见本文档的开发教程章节

## 4：打包发布
按常规的Java工程打包。

根据实际运行环境，如本地服务器、云主机、docker等具体情况，将Jar包发布到运行环境，启动运行。

项目的主入口是com.webank.truora.Application

连接区块链和数据库需修改以下配置文件,可参考工程里的模板，根据实际情况进行修改
```
application.yml,application-fiscobcos2.yml,application-fiscobcos3.yml,bcos3sdk_config.yml
```
数据库可采用Mysql/MariaDB，首先要先建库，保证配置正确，以连接到数据库。建表脚本为
```dbscripts/V2022.10__v1.0.0_init_table.sql```


```
Truora-Service适配fisco bcos3.x的版本,未提供docker/web等工具，如有需要可自行开发适配。
如使用适配fisco-bcos2.x的版本，可以在 github和readthedoc上都切换到v2stable分支，按该分支的代码和文档开发和操作。
```