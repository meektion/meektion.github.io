---
title: "让写blog的人专注于blog，实现全自动issue_blog，use：Issue + Hexo + Github Action"
date: 2024-12-23
categories: 
    - Top
    - Github
cover: https://gcore.jsdelivr.net/gh/WQhuanm/Img_repo_1@main/img/202412231400807.png
---


~~关于我吝啬(不愿买服务器)又懒惰(写博客不想太麻烦)，所以在全网网罗众多blog搭建idea，最后形成这篇blog这件事~~
 
>本文主要借鉴于Github的一个issue_blog项目
[yihong0618/gitblog](https://github.com/yihong0618/gitblog)

### 最终效果可参考我的[Blog仓库](https://github.com/WQhuanm/Issue_Blog)以及[Blog网站](https://wqhuanm.github.io/Issue_Blog/)

建立个人博客网站是在网上混迹的不错方式

相信有些人会在issue上记录自己的点滴，提交issue非常方便可惜缺少美观界面，也有些人使用hexo等静态网站，但是hexo等上传blog确实有些麻烦。

集思广益后，个人认为这份只需要用issue提交blog，通过Github Action来自动生成Readme,并自动部署博客到静态网站的方法也许值得一试。

### 一，配置Blog repo：clone这个[仓库](https://github.com/WQhuanm/Issue_Blog.git)到你想用来建立博客的仓库

这份博客运作的核心有三:
1. main.py: 内含生成readme的逻辑
2. generate_readme.yml: 通过action自动调用main.py生成readme
3. generate_page.yml: 在readme生成后，将博客部署到Gitpage网站

#### 必须执行的操作(require)
##### 1. 获取你的[Github Token](https://github.com/settings/tokens)
      
![](https://gcore.jsdelivr.net/gh/WQhuanm/Img_repo_1@main/img/202412231319184.png)

可以给你的token备注个名字(Note)。
有效期建议无限，否则每次过期你需要重新替换。
后面权限建议全部勾选，以防workflow权限不够。

![](https://gcore.jsdelivr.net/gh/WQhuanm/Img_repo_1@main/img/202412231340696.png)

最后Generate token便可获取，注意保存这个字符串，只会出现这一次

![](https://gcore.jsdelivr.net/gh/WQhuanm/Img_repo_1@main/img/202412231344362.png)

##### 2. 在你博客仓库的settings的Actions secrets and variables添加你刚刚获得的token。
建议命名为G_T(否则需要把2个workflow里面的G_T改为你设定的名字)

![](https://gcore.jsdelivr.net/gh/WQhuanm/Img_repo_1@main/img/202412231356327.png)

##### 3.在你博客仓库的settings的Actions ：General里运行workflow的读写权限

![](https://gcore.jsdelivr.net/gh/WQhuanm/Img_repo_1@main/img/202412231359958.png)

##### 4. 删除Blog文件夹
因为里面是我博客的存档(😓)


#### Option
1. 更改main.py的MD_HEAD内容，这个会写在readme头部


#### 执行后上述操作，你以后每次写issue(可以打上相关的label),都会更新到readme作为一个索引目录了。
readme会根据你打的label对你的博客进行分类

### 二，配置 Github Page
[yihong0618/gitblog](https://github.com/yihong0618/gitblog)是使用Zola配置了Github Page。我后面也会简要介绍一下zola的配置
我这里提供一种hexo的配置思路，采用的主题是[kira](https://github.com/ch1ny/kira-hexo)

#### 1. 你可以根据你选择的hexo主题进行本地配置后，把这个主题上传到你的私人仓库方便后面workflow来clone
 + 强烈建议**私人**仓库，因为你配置文件极可能有个人敏感信息
 + 如果你也想使用kira主题，你可以执行如下
    ```shell
    npm install -g hexo-cli
    npm create kira-hexo@latest myblog #虽然官方推荐用pnpm，但是我个人使用pnpm部署不成功，所以分享npm的方法
    #请删除myblog下的node_modules文件夹，避免可能的错误
    npm install 
    hexo clean
    hexo g
    hexo s
    ```
    这几步下去你的网站基本就配置好了，然后个性化可以参考官方文档修改_config.kira.yml和_config.yml（注意修改_config.yml的url为你Gitpage的url,参考generate_page的BASE_URL）

#### 2. 修改 .github\workflows\generate_page.yml，参考里面的注释
 + 修改BASE_URL
 + 修改jobs:deploy:steps:name: Generate Hexo public 里面的克隆仓库

#### 配置后，如果你的博客仓库提交过issue写文章，点击https://{username}.github.io/{your blog repo}/ 就可以看到你的网站了

#### Option
 + workflow生成page时，已经对网站的文件进行压缩，如果网站图片不是那么多的话，无梯子下访问速度还是很快的，如果你有很多图片上传的需求，推荐你使用国内主流图床或者使用Github做图床+CDN技术来优化，可以参考[jsDelivr和Github配合才是最佳免费CDN，五分钟学会使用，附搭建免费图床教程](https://blog.csdn.net/weixin_44786530/article/details/129851540)

#### 使用Zola简易配置Gitpage
 1. 你无需弄一个私有仓库了，只需要在博客仓库根目录添加一个config.toml文件，内容如下:
 ``` toml
    base_url = "https://{username}.github.io/{your blog repo}/" #如果repo是{username}.github.io,请改为https://{username}.github.io/
    generate_feeds = true
    feed_filenames = ["rss.xml"]
    theme = "even"
    taxonomies = [
        {name = "categories", feed = true},
        {name = "tags", feed = true},
    ]
    [extra]
    even_title = "yihong0618's Blog"
    even_menu = [
        {url = "$BASE_URL", name = "Home"},
        {url = "$BASE_URL/tags/top/", name = "Top"},
        {url = "$BASE_URL/issue-282/", name = "About"},
    ]
 ```
 2. generate_page.yml改为这个
 ``` yml
    name: Deploy static content to GitPages
    on:
    workflow_dispatch:
    workflow_run:
        workflows: [Generate GitBlog README]  # Depends on the completion of the workflow: Generate GitBlog README
        types:
        - completed
    # Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
    permissions:
    contents: read
    pages: write
    id-token: write
    concurrency:
    group: ${{ github.workflow }}
    cancel-in-progress: true
    jobs:
    deploy:
        environment:
            name: github-pages
            url: ${{ steps.deployment.outputs.page_url }}
        runs-on: ubuntu-latest
        env:
            GH_TOKEN: ${{ github.token }}
            ISITE_VERSION: v0.1.3
            ZOLA_VERSION: v0.19.2
            USER: ${{ github.repository_owner }}
            REPO: ${{ github.event.repository.name }}
             #if you use Gitpage and your repo'name is not {usernmae}.github.io,please use this
            BASE_URL: https://${{ github.repository_owner }}.github.io/${{ github.event.repository.name }}
            # else if your repo's name is {usernmae}.github.io,please use this,please use this
            # BASE_URL: https://${{ github.repository_owner }}.github.io/${{ github.event.repository.name }}
        steps:
        - name: Checkout
            uses: actions/checkout@v4
        - name: Generate markdown
            run: |
            gh release download $ISITE_VERSION --repo kemingy/isite -p '*Linux_x86_64*' --output isite.tar.gz
            tar zxf isite.tar.gz && mv isite /usr/local/bin
            isite generate --user $USER --repo $REPO
            gh release download $ZOLA_VERSION --repo getzola/zola -p '*linux*' --output zola.tar.gz
            tar zxf zola.tar.gz && mv zola /usr/local/bin
            cp config.toml output/config.toml
            cd output && zola build --base-url $BASE_URL
        - name: Setup Pages
            uses: actions/configure-pages@v4
        - name: Upload artifact
            uses: actions/upload-pages-artifact@v2
            with:
            path: 'output/public'
        - name: Deploy to GitHub Pages
            id: deployment
            uses: actions/deploy-pages@v3
 ```
> 参考文章:
[这个博客开源了](https://github.com/yihong0618/gitblog/issues/177)
[使用 GitHub Issues 来写博客，真香。](https://xie.infoq.cn/article/f89ea3ba86724ef568880ad04)
[jsDelivr和Github配合才是最佳免费CDN，五分钟学会使用，附搭建免费图床教程](https://blog.csdn.net/weixin_44786530/article/details/129851540)
