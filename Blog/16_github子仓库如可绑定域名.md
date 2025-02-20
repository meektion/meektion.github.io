---
title: github子仓库如可绑定域名
date: 2025-02-16 16:08:21
categories: 
    - Bookmark
tags: 
    - 一些小想法
cover: https://pic.superbed.cc/item/67b20d5dfa9f77b4dcf39cbd.png
---


要将GitHub子仓库绑定到自定义域名，请按照以下步骤操作：
<!---more--->
创建GitHub Pages：

在子仓库中创建一个名为 gh-pages 的分支。
将静态网站文件（例如 index.html）推送到 gh-pages 分支。
配置GitHub Pages：

进入子仓库的设置页面（Settings）。
在左侧菜单中选择“Pages”。
在“Source”部分，选择 gh-pages 分支作为 GitHub Pages 的源。
添加自定义域名：

在“Custom domain”字段中输入你的自定义域名，例如 example.com。
点击“Save”保存设置。
配置DNS记录：

登录你的域名注册商账户。
添加或更新 CNAME 记录，将你的自定义域名指向 yourusername.github.io。
如果你使用的是 A 记录，请将其指向以下 IP 地址：
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
启用HTTPS：

返回 GitHub Pages 设置页面，确保“Enforce HTTPS”选项已启用。
完成以上步骤后，你的 GitHub Pages 子仓库应该会绑定到自定义域名。
