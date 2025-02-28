---
title: 通过手机或平板等设备配置Hexo
date: 2025-02-28 15:14:05
categories: 
    - myblog
tags: 
    - 2025
    - 记录
cover: https://pic.superbed.cc/item/67c1d275f688033adbaa8dba.jpg
---


没有电脑时，可以通过手机或平板等设备配置Hexo。以下是具体步骤：

<!--more-->
### 1. 安装Termux（Android）
Termux是一个Android终端模拟器，支持Linux环境。

- 从Google Play或F-Droid下载并安装Termux。

### 2. 安装Node.js和Git
在Termux中执行以下命令：

```bash
pkg update
pkg install nodejs git
```

### 3. 安装Hexo
安装Hexo CLI：

```bash
npm install -g hexo-cli
```

### 4. 创建Hexo项目
初始化Hexo项目：

```bash
hexo init my-blog
cd my-blog
npm install
```

### 5. 配置Hexo
编辑`_config.yml`文件，设置博客标题、描述等信息：

```bash
nano _config.yml
```

### 6. 生成和预览
生成静态文件并启动本地服务器：

```bash
hexo generate
hexo server
```

在浏览器中访问`http://localhost:4000`预览博客。

### 7. 部署到GitHub Pages（可选）
安装部署插件：

```bash
npm install hexo-deployer-git --save
```

配置`_config.yml`：

```yaml
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: main
```

部署：

```bash
hexo deploy
```

### 8. 使用Markdown编辑器
在手机上使用Markdown编辑器（如iA Writer、JotterPad）编写文章，保存到`source/_posts`目录。

### 9. 同步代码（可选）
使用Git同步代码到GitHub或其他平台，方便在其他设备上继续工作。


