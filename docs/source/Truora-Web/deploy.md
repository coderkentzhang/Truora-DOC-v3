# 源码部署

## 依赖环境

| 环境  | 版本               |
| ----- | ------------------ |
| Nginx | Nginx 1.6 或以上版本 |

关于 `Nginx` 安装，请参考 [附录--安装 Nginx](../../Truora-Service/appendix.html#install_nginx)

## 安装

* 拉取代码：

```Bash
# 拉取代码
git clone https://github.com/WeBankBlockchain/Truora-Web.git;
    
# 进入目录
cd Truora-Web
```

* 拷贝 `Nginx` 配置文件
    
```Bash
# 拷贝项目的 Nginx 配置
cp docker/truora-web.conf  /etc/nginx/conf.d
```

```eval_rst
.. admonition:: 提示

    - 如果主机中已经存在 Nginx，只需要把 `docker/truora-web.conf` 拷贝到 Nginx 的配置文件（默认：`/etc/nginx/nginx.conf`）中 `include` 的目录下即可。
``` 

```eval_rst
.. important::

    - Nginx 的配置文件直接拷贝 `docker/truora-web.conf` 文件即可，**不建议** 修改 Nginx 的主配置文件（默认：`/etc/nginx/nginx.conf`）
```


* 拷贝源码到 `Nginx` 项目目录


```Bash
# 备份原有 index.html 文件
mv /usr/share/nginx/html/index.html /usr/share/nginx/html/index.html.back

# 拷贝项目文件
cp -r dist/* /usr/share/nginx/html
```

## 启动 Nginx

* 启动 `Nginx`

```Bash
# 启动 Nginx
nginx 
```

* 检查启动结果

```Bash
# 检查进程
$ ps -ef | grep -i nginx

root     31462     1  0 17:33 ?        00:00:00 nginx: master process nginx
www-data 31562 31462  0 17:37 ?        00:00:00 nginx: worker process
www-data 31563 31462  0 17:37 ?        00:00:00 nginx: worker process

# 检测端口
$ netstat -anp | grep -i listen| grep -i tcp|grep 5020

tcp        0      0 0.0.0.0:5020            0.0.0.0:*               LISTEN      31462/nginx: master
```

如果能查看到 `nginx` 的进程，同时 `5020` 端口也被 `Nginx` 服务监听，那么 **启动成功**。

* 浏览器访问
    
    打开浏览器，输入 `http://[IP]:5020`，比如：`http://127.0.0.1:5020`

```eval_rst
.. admonition:: 提示

     - 注意替换服务器的 `IP` 地址
```

## 错误排查
* 查看 `Nginx` 日志：

```Bash
# 查看 Nginx 的错误日志
$ cat /var/log/nginx/error.log
```

* 查看项目的 `Nginx` 日志：

```Bash
$ 项目的 error 日志
cat /var/log/nginx/oracle-error.log

$ 项目的 access 日志
cat /var/log/nginx/oracle-access.log
```

