# gelove.github.io

## 安装 zola

```sh
# MacOS
brew install zola
# linux
curl -sL https://github.com/getzola/zola/releases/download/v0.19.2/zola-v0.19.2-x86_64-unknown-linux-gnu.tar.gz | tar xz -C /usr/local/bin
```

## 开发与构建

```sh
# 创建本地服务
zola server
# 构建生成静态文件
zola build
```

> 注意: 将工作流权限更改为可读写, 以便 GitHub Actions 工作流自动部署

在仓库中, 点击 Settings -> Actions -> General -> Workflow permissions, 选择 `Read and write permissions` 并保存
