---
title: Docker部署xxl-job步骤
date: 2024-03-14 13:09:17
tags:
---

### 使用Docker命令创建自定义网络
```shell
docker network create hdzl_net
```

### 使用Docker命令启动MySQL容器
```shell
docker run -p 3306:3306 \
--name mysql \
-e MYSQL_ROOT_PASSWORD=qq490983587 \
--network hdzl_net \
-d mysql
```

### 在MySQL数据库中执行xxl-job数据库脚本
在宿主机上使用连接mysql执行xxl-job-admin项目中提供的`tables_xxl_job.sql`脚本
```text
主机地址：localhost
端口：3306
用户名：root
密码：qq490983587
```

### 使用Docker命令启动xxl-job-admin
```shell
docker run \
-e PARAMS="--spring.datasource.url=jdbc:mysql://mysql:3306/xxl_job?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai --spring.datasource.username=root --spring.datasource.password=qq490983587" \
-p 8080:8080 \
-v /Users/shiyanjun/logs:/data/applogs \
--network=hdzl_net \
--name xxl-job-admin \
-d xuxueli/xxl-job-admin:2.4.0
```

### 浏览器访问xxl-job-admin
打开浏览器访问：http://localhost:8080/xxl-job-admin/
登录用户名：admin
登录密码：123456
 
