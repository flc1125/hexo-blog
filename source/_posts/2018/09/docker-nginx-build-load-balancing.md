---
title: docker nginx搭建负载均衡
date: 2018-09-07 18:24:34
updated: 2018-09-07 18:24:39
categories: 
    - 运维
---

> 基于多个 Docker 容器

## 拉取 nginx 镜像

```bash
docker pull nginx:alpine # 推荐 alpine
```

## 容器间通信

官方提供的是 `docker network` 作为解决方案

```bash
docker netware create nginx # 创建名称 nginx 网络
```

## 创建容器

> 负载均衡，所以至少需要三个 nginx 容器

- 负载均衡器

    ```bash
    docker run -dit -v /nginx/:/nginx/ -p 80:80 --network nginx --network-alias master --name nginx-master nginx:alpine
    # 挂载目录仅演示目的
    ```
    
    说明：
    
    - `--network nginx` 使用 `nginx` 网络
    - `--network-alias master` 网络中的别名
    - **宿主机对应 `80` 端口**

- WEB节点A

    ```bash
    docker run -dit -v /nginx/:/nginx/ -p 8001:80 --network nginx --network-alias slave1 --name nginx-slave1 nginx:alpine
    # 8001 自定义，不重复即可
    ```
    
- WEB节点B

    ```bash
    docker run -dit -v /nginx/:/nginx/ -p 8002:80 --network nginx --network-alias slave2 --name nginx-slave2 nginx:alpine
    # 8002 自定义，不重复即可
    ```

## nginx 配置

> 仅列出核心部分代码

- 负载均衡器

    ```nginx
    upstream web {
        server slave1:80 weight=3;  # weight 为权重
        server slave2:80 weight=1;
    }
    
    location / {
       proxy_pass   http://web;
    }
    ```

- WEB节点A/B

    ```
    location / {
        root   /data/html;
        index  index.html index.htm;
    }
    ```