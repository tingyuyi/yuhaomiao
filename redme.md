# 宇浩渺AI探索

这是一个使用Hugo搭建的个人博客，专注于AI技术的探索与分享。

## 特点

- 使用Hugo静态站点生成器
- 采用Stack主题
- 支持文章搜索
- 响应式设计
- 支持暗色模式

## 本地开发

1. 克隆仓库：
```bash
git clone https://github.com/[你的GitHub用户名]/[你的仓库名].git
cd [你的仓库名]
```

2. 安装Hugo（如果还没有安装）：
- Windows: `choco install hugo-extended`
- Mac: `brew install hugo`
- Linux: `sudo apt-get install hugo`

3. 安装主题：
```bash
git submodule update --init --recursive
```

4. 启动本地服务器：
```bash
hugo server -D
```

5. 访问 http://localhost:1313 查看站点

## 添加新文章

```bash
hugo new content/post/your-post-name.md
```

## 部署

1. 创建 `.github/workflows/hugo.yml` 文件，添加以下内容：

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true
      - name: Build
        run: hugo --minify
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

2. 在GitHub仓库设置中启用GitHub Pages，选择GitHub Actions作为部署源。

3. 推送代码到main分支，GitHub Actions将自动构建并部署网站。

## 许可证

MIT