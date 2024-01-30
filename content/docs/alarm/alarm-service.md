---
title: "Alarm Server"
description: ""
summary: ""
date: 2024-01-29T23:25:38+08:00
lastmod: 2024-01-29T23:25:38+08:00
draft: false
menu:
  docs:
    parent: ""
    identifier: "alarm-498d513f08748d2e5bb2f86be4634b1e"
weight: 1
toc: true
seo:
  title: "alarm" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

# 服务器部署文档



> [!IMPORTANT]
> ✨  程序思路和部分代码均来自 [BARK](https://github.com/Finb/bark-server)
>
> ✨  一个基于golang的推送服务后端，主要用于推送消息给客户端。
>
> ✨  重新实现了[BARK](https://github.com/Finb/bark-server)的接口，采用了gin框架，方便后续扩展。



### 配置文件

```yaml
system:
  name: "AlarmPawServer"
  user: ""         # 用户名 非必填 获取info信息时使用
  password: ""    # 密码  非必填
  host: "0.0.0.0"  # 服务监听地址
  port: "8080"   # 服务监听端口 docker-compose中的端口映射必须与此端口一致
  mode: "release"   # debug,release,test
  dbType: "default" # default,mysql 
  dbPath: "/data" # 数据库文件存放路径 

mysql: # 仅在 dbType: "mysql" 时有效
  host: "localhost"
  port: "3306"
  user: "root"
  password: "root"

apple: # 复制项目中的配置，不需要修改，仅在自己编译app时需要修改
  keyId:
  teamId:
  topic:
  apnsPrivateKey:

```


  ### 手动编译部署
 * 配置文件 config.yaml 要保存在 data 下，否则无法启动
 * 自己编译可以 通过 -c 指定配置文件。
 * 目录结构:
    - /deploy
      - main # 主程序
      - /data
        - config.yaml # 配置文件

```shell
#  CGO_ENABLED=0 GOOS=linux GOARCH=amd64 用来指定服务器结构
#  go build -o  main 第一个 main 是路径 指定存放位置和文件名 /delploy/main
  CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o  main  main.go || echo "编译linux版本失败"
```
上传文件到服务器上，然后执行以下命令即可启动服务。
```shell
# 默认配置文件
  ./main
# 指定配置文件
  ./main -c /custom/config.yaml
```




## Docker部署
```shell
  docker run -d --name alarm-paw-server -p 8080:8080 -v ./data:/data  --restart=always  thurmantsao/alarm-paw-server:latest
```

## Docker-compose部署
* 复制项目中的/deploy文件夹到服务器上，然后执行以下命令即可。
* 必须有/data/config.yaml 的配置文件，否则无法启动，文件中的配置项，可以根据自己的需求进行修改。

### 启动
```shell
  docker-compose up -d
```


