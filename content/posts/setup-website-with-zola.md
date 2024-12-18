+++
title = "使用 Zola 和 Github Pages 搭建网站"
date = 2022-09-05T21:05:00+08:00
type = "post"
description = "使用 Zola 作为静态网站生成器搭建个人网站和博客，并使用 Github Actions 部署到 Github Pages。"
[taxonomies]
tags = ["Zola", "Github"]
+++

使用 Zola 作为静态网站生成器搭建个人网站和博客，并使用 Github Actions 部署到 Github Pages。

Zola 是一个用 Rust 编写的静态网站生成器，使用动态模板将 Markdown 格式的内容转换为静态 HTML 页面。使用 Github Actions 部署到 Github Pages 是 GitHub 上个月刚刚发布的新功能。

### 新建 Github 仓库

首先创建一个名为 `<username>.github.io` 的仓库：

![create repository](/images/1697204176980.jpg)

新建出来的是一个空的仓库，把它 clone 到本地：

```sh
git clone https://github.com/<username>/<username>.github.io
```

接下来的所有操作都在这个文件夹内进行：

```sh
cd <username>.github.io
```

### 安装 Zola

使用以下命令安装 Zola。

#### macOS

```sh
brew install zola
```

#### Linux

```sh
curl -sL https://github.com/getzola/zola/releases/download/v0.19.2/zola-v0.19.2-x86_64-unknown-linux-gnu.tar.gz | tar xz -C /usr/local/bin
```

### 使用 Zola

1. 运行命令 `zola init` 进行初始化。
   zola init 完会创建配置文件 config.toml 和几个文件夹:

   - content/ 放所有的文章
   - sass/ 如字面意义放 sass
   - templates/ 下面放 Tera 模板，即 Zola 使用的 HTML 模板引擎。
   - content/ 放我们的文章
   - static/ 图片等静态资源
   - themes/ 主题

2. 根据文档说明进行定制主题、模版。
3. 补充正文内容。
4. 运行命令 `zola serve` 进行本地预览，在浏览器中打开 `http://127.0.0.1:1111` 就可以看到网站了。

### 配置 Github Actions

创建 Github Actions 工作流文件 `.github/workflows/deploy.yml`。此工作流程负责构建网站内容并发布到 `gh-pages` 分支。

> 注意: 将工作流权限更改为可读写, 以便 GitHub Actions 工作流自动发布内容到 `gh-pages` 分支。
> 在仓库中, 点击 Settings -> Actions -> General -> Workflow permissions, 选择 `Read and write permissions` 并保存

```yml
on:
  push:
    branches:
      - main

name: Build and deploy GH Pages
jobs:
  build:
    runs-on: ubuntu-22.04
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Deploy
        uses: shalzz/zola-deploy-action@v0.19.2
        env:
          # Target branch
          PAGES_BRANCH: gh-pages
          BUILD_DIR: .
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

这个工作流程文件执行两项作业：

1.  `actions/checkout` 下载仓库的副本。
2.  `zola-deploy-action` 构建页面文件并发布到 `gh-pages` 分支。

### 推送代码

```sh
git add --all
git commit -m "Initial commit"
git push -u origin main
```

第一次提交代码，会触发运行 Github Actions 的工作流程。等待工作流程运行完成之后，网页代码会发布到 `gh-pages` 分支。

### 部署

进入 Github 仓库的 `Settings > Pages` 页面，在 `Build and deployment` 下选择 `gh-pages` 分支进行构建和部署。
![source](/images/1734496288602.jpg)

等一会你的网站就可以访问了：`https://<username>.github.io`。

### 自定义域名

参考[文档](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)配置自定义域名的 DNS 记录。

进入 Github 仓库的 `Settings > Pages` 页面，设置自定义域名。
比如说我的自定义域名 `allens.top` 设置成功后，即可访问：[`https://allens.top`](https://allens.top)。
![custom domain](/images/1697204305581.jpg)

这将创建一个提交，将 CNAME 文件直接添加到分支的根目录下。

> 每次更新时都会重新创建 `gh-pages`分支，CNAME 文件也会消失，导致域名无法正常解析。
> 所以需要在添加域名解析后，将 CNAME 文件添加到本地仓库主分支。zola 需要将 CNAME 添加到 static 目录下，以便构建时将其放在公共文件夹的根目录。

### 总结

我的网站完全按照以上步骤搭建，请访问：[gelove/gelove.github.io](https://github.com/gelove/gelove.github.io) 查看源代码。

这次我的网站搭建过程，也是一个很好的学习实践。我学习到了：

- Zola
- Github Pages
- Github Actions

### 参考资料

- [Zola Docs](https://www.getzola.org/documentation/)
- [GitHub Actions starter workflows](https://github.com/actions/starter-workflows/tree/main/pages)
