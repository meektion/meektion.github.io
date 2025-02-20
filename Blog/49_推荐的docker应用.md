---
title: 推荐的docker应用
date: 2025-02-20 14:18:19
categories: 
    - tool
tags: 
    - 2025
    - 记录
cover: https://pic.superbed.cc/item/67b7352b01c078e310534188.png
---


# 推荐的docker应用
> - [portainer（容器管理工具）](#portainer容器管理工具)
> - [青龙面板](#青龙面板)
> - [Photopea](#photopea)
> - [stirling PDF](#stirling-pdf)
> - [Halo博客](#halo博客)
> - [jellyfin影音](#jellyfin影音)
> - [红白机游戏](#红白机游戏)
> - [dos游戏](#dos游戏)
> - [excalidraw(手绘风画板)](#excalidraw)
> - [memos](#memos)
> - [思维导图](#simple-mind-map)
> - [证件照、人像抠图](#hivisionidphotos)

<!--more-->

## [portainer（容器管理工具）](https://www.portainer.io/)

- 命令行：

```
docker run -d -p 9000:9000 \
--restart=always \
-v /var/run/docker.sock:/var/run/docker.sock \
--name portainer-test \
docker.io/portainer/portainer:latest
```
- docker compose

```
version: "3"
services:
  portainer:
    image: portainer/portainer:latest
    container_name: portainer
    ports:
    - "9000:9000"
    volumes:
    - /app/portainer/data:/data
    - /var/run/docker.sock:/var/run/docker.sock
```

## [青龙面板](https://github.com/whyour/qinglong)

- 命令行：

```
docker run -dit \
-v /QL/jd/config:/ql/config \
-v /QL/jd/log:/ql/log \
-v /QL/jd/scripts:/ql/scripts \
-v /QL/jd/jbot:/ql/jbot \
-v /QL/jd/db:/ql/db \
-p 5700:5700 \
-e ENABLE_HANGUP=true \
-e ENABLE_WEB_PANEL=true \
--name jdqinglong \
--hostname jdqinglong \
--restart always \
whyour/qinglong
```
- docker compose

```
version: '2'
services:
  web:
    # alpine 基础镜像版本
    image: whyour/qinglong:latest
    # debian-slim 基础镜像版本
    # image: whyour/qinglong:debian  
    volumes:
      - ./data:/ql/data
    ports:
      - "0.0.0.0:5700:5700"
    environment:
      # 部署路径非必须，以斜杠开头和结尾，比如 /test/
      QlBaseUrl: '/'
    restart: always
```

## [Photopea](https://github.com/photopea/photopea)

- 命令行：

```
docker run -d --name ps -p 2887:2887 registry.cn-guangzhou.aliyuncs.com/welldene/docker:photopea
```
- docker compose

```
version: '3.9'
services:
    welldene:
        image: 'registry.cn-guangzhou.aliyuncs.com/welldene/docker:photopea'
        ports:
            - '2887:2887'
        container_name: photopea
```

## [stirling PDF](https://github.com/Stirling-Tools/Stirling-PDF)

- 命令行：

```
docker run -d \
  -p 8080:8080 \
  -v ./trainingData:/usr/share/tessdata \
  -v ./extraConfigs:/configs \
  -v ./logs:/logs \
  -e DOCKER_ENABLE_SECURITY=false \
  -e INSTALL_BOOK_AND_ADVANCED_HTML_OPS=false \
  -e LANGS=zh_CN \
  --name stirling-pdf \
  frooodle/s-pdf:latest
```
- docker compose

```
version: '3.9'
services:
  stirling-pdf:
    image: registry.cn-hangzhou.aliyuncs.com/jeson/s-pdf:latest
    container_name: Stirling-PDF
    ports:
      - '6080:8080'
    volumes:
      - /data/s-pdf/trainingData:/usr/share/tessdata    # OCR 语言支持
      - /data/s-pdf/extraConfigs:/configs
      - /data/s-pdf/customFiles:/customFiles/
      - /data/s-pdf/logs:/logs/
    environment:
      DOCKER_ENABLE_SECURITY: true    # 启用内部安全功能
      SECURITY_ENABLELOGIN: true    # 启用登录功能
      SECURITY_INITIALLOGIN_USERNAME: jeven    # Stirling-PDF登录账号
      SECURITY_INITIALLOGIN_PASSWORD: jeven    # Stirling-PDF登录密码
      UI_APPNAME: jeven-PDF    # 自定义导航标题
      INSTALL_BOOK_AND_ADVANCED_HTML_OPS: false
      LANGS: zh_CN    # 设置语言
```

## [Halo博客](https://github.com/halo-dev/halo)
**[Halo官网](https://www.halo.run)**

- 命令行：

```
docker run -it -d --name halo -p 8090:8090 -v ~/.halo2:/root/.halo2 halohub/halo:2.16
```
- docker compose

```
version: "3"
services:
  halo:
    image: registry.fit2cloud.com/halo/halo:2.16
    restart: on-failure:3
    volumes:
      - ./halo2:/root/.halo2
    ports:
      - "8090:8090"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8090/actuator/health/readiness"]
      interval: 30s
      timeout: 5s
      retries: 5
      start_period: 30s          
    command:
      # 外部访问地址，请根据实际需要修改
      - --halo.external-url=http://your-IP-address:8090/
```

## jellyfin影音

- 命令行：

```
docker run -d -p 8096:8096 -v /jellyfin/config:/config -v /media/nas/电影:/media jellyfin/jellyfin:latest
```
- docker compose

```
version: "3"
services:
```

## 红白机游戏

- 命令行：

```
docker run --restart always -p 8081:80 --name jsnes -d swr.cn-north-4.myhuaweicloud.com/jeven/jsnes:1.0.0
```
- docker compose

```
version: "3"
services:
```

## dos游戏

- 命令行：

```
docker run -d --name dosgame -p 262:262 oldiy/dosgame-web-docker:latest
```
- docker compose

```
version: "3"
services:
```

## [excalidraw](https://juejin.cn/post/7264503091117080628)

- 命令行：

```
docker run
```
- docker compose

```
version: "3"

services:
  excalidraw:
    image: fanjunyang/jy-excalidraw # 默认使用latest的TAG，也就是我构建的最新镜像
    healthcheck:
      disable: true
    ports:
      - "8021:80" # 端口可自行修改
    environment:
      BACKEND_V2_GET_URL: https://example.com/excalidraw-storage-backend/api/v2/scenes/
      BACKEND_V2_POST_URL: https://example.com/excalidraw-storage-backend/api/v2/scenes/
      LIBRARY_URL: https://libraries.excalidraw.com
      LIBRARY_BACKEND: https://us-central1-excalidraw-room-persistence.cloudfunctions.net/libraries
      SOCKET_SERVER_URL: https://example.com/
      STORAGE_BACKEND: "https" # 协议是 https
      HTTP_STORAGE_BACKEND_URL: "https://example.com/excalidraw-storage-backend/api/v2"

  excalidraw-storage-backend:
    image: kiliandeca/excalidraw-storage-backend
    ports:
      - "8022:8080"
    environment:
      STORAGE_URI: redis://redis:6379

  excalidraw-room:
    image: excalidraw/excalidraw-room
    ports:
      - "8023:80"

  redis:
    image: redis
    ports:
      - "6379:6379"
```


## [memos](https://github.com/usememos/memos)

- 命令行：

```
docker run -d --name memos -p 5230:5230 -v ~/.memos/:/var/opt/memos neosmemo/memos:stable
```
- docker compose

```
version: 
services:
```


## [simple-mind-map](https://wanglin2.github.io/mind-map-docs/start/deploy.html#docker)

- 命令行：

```
docker run -d --name simple-mind-map  -p 8081:8080 shuiche/mind-map:latest
```
- docker compose

```
version: 
services:
```


## [HivisionIDPhotos](https://github.com/Zeyi-Lin/HivisionIDPhotos?tab=readme-ov-file#-docker-%E9%83%A8%E7%BD%B2)

- 命令行：

```
docker run -d -p 7860:7860 linzeyi/hivision_idphotos
```
- docker compose

```
version: 
services:
```
