title: MongoDB的基本使用
author: hero576
toc: true
tags:
  - MongoDB
categories:
  - 数据库
date: 2018-04-08 17:28:00
---
> 
<!-- more -->


[官方文档链接](https://docs.mongodb.com/manual/introduction/)
## 安装和配置
### linux

1. 下载完安装包，并解压 tgz（以下演示的是 64 位 Linux上的安装）。
```shell
curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.6.tgz # 下载
tar -zxvf mongodb-linux-x86_64-3.0.6.tgz # 解压
mv  mongodb-linux-x86_64-3.0.6/ /usr/local/mongodb # 将解压包拷贝到指定目录
 ```
2. MongoDB 的可执行文件位于 bin 目录下，所以可以将其添加到 PATH 路径中：  
```shell
export PATH=<mongodb-install-directory>/bin:$PATH
```
mongodb-install-directory 为 MongoDB 的安装路径。如本文的 /usr/local/mongodb 。  
3. 创建数据库目录  
MongoDB的数据存储在data目录的db目录下，但是这个目录在安装过程不会自动创建，所以你需要手动创建data目录，并在data目录中创建db目录。以下实例中我们将data目录创建于根目录下(/)。  
__注意：__/data/db 是 MongoDB 默认的启动的数据库路径(--dbpath)。
```shell
mkdir -p /data/db
```
4. 命令行中运行 MongoDB 服务
 - 可以在 /usr/local/mongodb/bin/ 目录下使用命令：　　
```shell
./mongod
```
 - 在 /usr/local/mongodb/ 下添加conf目录，并添加mongodb.conf配置文件。
```shell
./mongod -f /usr/local/mongodb/conf/mongodb.conf 　　或者
./mongod --config /usr/local/mongodb/conf/mongodb.conf
```
mongodb.conf配置文件内容如下：
```shelldbpath=/data/db
bind_ip=127.0.0.1
port=27017
fork=true
logappend=true
shardsvr=true
pidfilepath=/usr/local/mongodb/mongo.pid
logpath=/usr/local/mongodb/log/mongodb.log
```  
5. 设置开机启动mongo  
 - 将/usr/local/mongodb/bin/mongod
  --dbpath /data/db
  --fork
  --port 27017 --logpath=/usr/local/mongodb/log/mongodb.log
  --logappend    
添加到 /etc/rc.local 中。