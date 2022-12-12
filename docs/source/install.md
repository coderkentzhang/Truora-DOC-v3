# 安装部署

Truora-Service本身是个典型的SpringBoot工程，只要熟悉Java以及相关的知识，则可以按照一般Java项目的开发部署模式，完成全流程的操作。


**当前版本聚焦于代码级的开发编译部署。未提供docker/web等工具，如有需要可自行开发适配。**

## 1：从github/gitee获取Truora-Service代码
```
git clone https://github.com/WeBankBlockchain/Truora-Service.git
git clone https://gitee.com/WeBankBlockchain/Truora-Service.git
```

```eval_rst 
.. important::

如区块链底层平台为FISCO BCOS 3.1.0+,使用Truora-Service的master分支。不支持FISCO  BCOS3.0.0版本。

如区块链底层平台为FISCO BCOS 2.6.0+(且低于3.0.0)，则使用Truora-Service的v2stabe分支
``` 


## 2：构建开发环境
结合自己的开发环境IDE导入Java项目

## 3：开发编译
参见本文档的开发教程章节

## 4：打包发布
按常规的Java工程打包。

根据实际运行环境，如本地服务器、云主机、docker等具体情况，将Jar包发布到运行环境。

1) 运行前应已经安装FISCO BCOS的底层并把链运行起来。FISCO BCOS底层的安装部署参见其[操作文档](https://fisco-bcos-doc.readthedocs.io/zh_CN/latest/)

2) 连接区块链和数据库需修改以下配置文件,可参考工程里的模板，根据实际情况进行修改
```
application.yml,application-fiscobcos2.yml,application-fiscobcos3.yml,bcos3sdk_config.yml
```

3) 数据库应事先安装。可采用Mysql/MariaDB，首先要先建库，保证配置正确，以连接到数据库。建表脚本为
```dbscripts/V2022.10__v1.0.0_init_table.sql```


4) WeBASE等中间件平台，按需自行安装。参见[操作文档](https://webasedoc.readthedocs.io/zh_CN/latest/)

最后通过脚本或命令行方式启动Truora-Service的java应用，可参考项目里附带的start.sh脚本

项目的主入口是com.webank.truora.Application




