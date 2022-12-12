
# 源码编译

## 安装介绍

Truora-Service本身是个典型的SpringBoot工程，只要熟悉Java以及相关的知识，则可以按照一般Java项目的开发部署模式，完成全流程的操作。


**当前版本聚焦于代码级的开发编译部署。未提供docker/web等工具，如有需要可自行开发适配。**

## 前置依赖

在使用本组件前，请确认系统环境已安装相关依赖软件，清单如下：

| 依赖软件 | 说明 |
| --- | --- | 
| FISCO-BCOS | >= 2.6.0 or >=3.1.0 | 
| MySQL | >= mysql-community-server[5.7] | 
| Java | JDK[1.8] | 
| Git | 下载的源码使用 Git | 

请参考：[附录](../appendix.md) 检查系统是否已经安装相关依赖软件。

*运行前应已经安装FISCO BCOS的底层并把链运行起来。FISCO BCOS底层的安装部署参见其[操作文档](https://fisco-bcos-doc.readthedocs.io/zh_CN/latest/)

*数据库应事先安装。可采用Mysql/MariaDB，首先要先建库，保证配置正确，以连接到数据库。建表脚本为项目文件路径里的
```dbscripts/V2022.10__v1.0.0_init_table.sql```

*WeBASE等中间件平台，按需自行安装。参见[操作文档](https://webasedoc.readthedocs.io/zh_CN/latest/)


## 拉取代码

执行命令：
```Bash
# 拉取源码
github:
```
git clone https://github.com/WeBankBlockchain/Truora-Service.git
```
gitee:
```
git clone https://gitee.com/WeBankBlockchain/Truora-Service.git
```

***
注意：

如区块链底层平台为FISCO BCOS 3.1.0+,使用Truora-Service的master分支。3.0.0版本底层未支持。

如区块链底层平台为FISCO BCOS 2.6.0+(且低于3.0.0)，则使用Truora-Service的v2stabe分支。低于2.6.0版本底层未支持。
***



# 进入目录
cd Truora-Service
```


## 编译代码

方式一：如果服务器已安装Gradle，且版本为 Gradle-5.60 +

```shell
gradle build -x test
```

方式二：如果服务器未安装 Gradle，或者版本低于 Gradle-4.10，使用 gradlew 编译

```shell
chmod +x ./gradlew && ./gradlew build -x test
```

构建完成后，会在根目录 Truora-Service 下生成已编译的代码目录 dist。



## 加密类型
FISCO-BCOS 链有两种类型： **非国密（ECDSA）** 和 **国密（SM2）** 。


```eval_rst
.. important::

    - 不同的连接方式，需要拷贝的证书不同
    - 使用 **非国密** 的方式连接节点，需要拷贝 `sdk/` 目录下 `ca.crt`、`node.crt` 和 `node.key` 文件
    - 使用 **国密** 的方式连接节点，需要拷贝 `sdk/gm` 目录下 `gm` 开头的所有文件
```



## 修改配置

进入 `dist` 目录

```
cd dist
```

`dist` 目录提供了一份配置模板 `conf`

### 配置数据库
修改配置 `conf/application.yml` 文件

```Bash
# 进入 conf 目录
cd conf
```

* 修改数据库 `IP` 地址，用户名和密码。 
   
```yaml
 datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/truora?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf-8&zeroDateTimeBehavior=convertToNull&useSSL=false
    username: "defaultAccount"
    password: "defaultPassword"
```  
 数据库应事先安装。可采用Mysql/MariaDB，首先要先建库，保证配置正确，以连接到数据库。建表脚本为项目文件路径里的
```dbscripts/V2022.10__v1.0.0_init_table.sql```



### 拷贝证书

在拷贝证书文件之前，需要确定使用哪种方式连接到链接点：非国密连接（ECDSA）还是 国密连接（SM2），请参考：[连接类型](./deploy.html#connection_type)

  * 非国密连接
  
```shell
# 进入 conf 目录
cd conf

# 非国密连接
cp  /${PATH_TO_SDK}/node.* .
cp  /${PATH_TO_SDK}/ca.crt .
```

```eval_rst
.. important::

    - **非国密链：** 拷贝节点所在目录 `nodes/${ip}/sdk` 下的 `ca.crt`、`node.crt` 和 `node.key` 文件拷贝到 `conf` 目录
```

  * 国密连接
  
    
```shell
# 进入 conf 目录
cd conf

# 国密连接
cp  /${PATH_TO_SDK}/gm/gm.* .
```

```eval_rst
.. important::

    - **国密链：** 拷贝节点所在目录 `nodes/${ip}/sdk/gm` 下的 `gm` 开头的所有文件拷贝到 `conf` 目录
```



### 配置连接


Truora-Service 支持同时连接多条链，以及连接同一条链中的多个群组。


* 如果连接FISCO BCOS 2.x版本的底层，配置application-fiscobcos2.yml。

* 如果连接FISCO BCOS 3.x版本的底层，配置application-fiscobcos3.yml，以及bcos3sdk_config.xml

配置文件有模板和注释，可参照修改。

**虽然项目支持一个Truora-Service连接多条链，但从工程实操上讲，不建议这么做，在配置复杂度，可用性等方面都会带来一些问题**

**建议是每个Truora-Service实例连接一条底层链。



## 服务启停

返回到 `dist` 目录执行：

* 启动

```Bash
# 采用非国密连接（ECDSA）
bash start.sh

# 采用国密连接（SM2），添加 gm 参数
bash start.sh gm
```

* 停止
```shell
bash stop.sh
```

* 检查

```shell
bash status.sh
```
**备注**：服务进程起来后，需通过日志确认是否正常启动，出现以下内容表示正常；如果服务出现异常，确认修改配置后，重启提示服务进程在运行，则先执行 `stop.sh`，再执行 `start.sh`。

```Bash
Application() - main run success...
```



## 查看日志

在 dist 目录查看：

```Bash
# 前置服务日志：
tail -f log/Oracle-Service.log
```
