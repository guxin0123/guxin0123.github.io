# Guxin 的个人博客

基于 [Jekyll](https://jekyllrb.com/) 与 [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 主题搭建的静态博客，
通过 GitHub Actions 自动构建并发布到 GitHub Pages：<https://guxin0123.github.io>

## 特性

- 响应式布局，支持浅色 / 深色 / 跟随系统三种配色
- 站内搜索、文章目录、阅读时长与置顶文章
- 分类与标签自动归档
- 支持 Mermaid 图表与 MathJax 数学公式
- 支持 PWA 安装
- 推送即自动部署，无需手动构建

## 目录结构

```text
.
├── _config.yml                 # 站点与主题配置
├── Gemfile                     # Ruby 依赖（Chirpy 主题、html-proofer）
├── index.html                  # 首页
├── _data/
│   ├── contact.yml             # 侧边栏联系方式
│   └── share.yml               # 文章底部分享平台
├── _plugins/
│   └── posts-lastmod-hook.rb   # 自动记录文章最后修改时间
├── _posts/                     # 文章（YYYY-MM-DD-标题.md）
├── _tabs/                      # 顶栏页面：分类、标签、归档、关于
├── assets/
│   ├── css/jekyll-theme-chirpy.scss  # 自定义样式入口
│   └── img/                    # 头像、封面与文章图片
└── .github/workflows/
    └── pages-deploy.yml        # 构建与部署工作流
```

## 本地开发

需要 Ruby 3.x 与 Bundler：

```bash
bundle install
bundle exec jekyll serve
```

然后访问 <http://127.0.0.1:4000>。修改内容后刷新页面即可，无需重启。

如果不想安装 Ruby，也可以使用 Dev Containers 或直接推送到 `main` 分支，由 GitHub Actions 构建。

## 部署

仓库使用 GitHub Actions 部署，**首次推送前**需要把 Pages 的构建来源切换为 Actions：

1. 打开 **Settings → Pages**
2. **Build and deployment → Source** 选择 **GitHub Actions**
3. 推送提交后，"Build and Deploy" 工作流会自动构建、检查站内链接并发布

之后每次向 `main` 分支推送（修改 README、LICENSE、.gitignore 除外）都会触发部署。
也可以在 **Actions** 页面手动运行 `Build and Deploy`。

工作流分为两个任务：

| 任务 | 内容 |
| --- | --- |
| build | 安装 Ruby 依赖 → 构建站点 → `html-proofer` 校验站内链接 → 上传产物 |
| deploy | 将产物发布到 GitHub Pages 环境 |

## 写一篇新文章

在 `_posts` 下新建 `YYYY-MM-DD-slug.md`：

```yaml
---
title: 文章标题
date: 2026-10-28 20:00:00 +0800
categories: [Blogging]
tags: [jekyll, tutorial]
description: 一句话摘要，用于搜索与分享预览。
---

正文内容……
```

常用可选字段：`pin: true`（置顶）、`math: true`（数学公式）、`mermaid: true`（图表）、
`image.path`（封面图）、`toc: false`（关闭目录）。

## 自定义

- **站点信息**：编辑 `_config.yml` 中的 `title`、`tagline`、`description`、`url` 与 `social`
- **社交入口**：在 `_data/contact.yml` 中增删项目
- **头像与图片**：替换 `assets/img/avatar.png`，文章图片放在 `assets/img/` 下并用 `/assets/img/...` 引用
- **样式**：在 `assets/css/jekyll-theme-chirpy.scss` 末尾追加自定义 CSS
- **评论**：在 `_config.yml` 的 `comments.provider` 中选择 `giscus`、`utterances` 或 `disqus`

## 常见问题

**页面样式丢失或链接 404？** 检查 `_config.yml` 中的 `url` 与 `baseurl` 是否与实际部署地址一致。
用户站点（`<用户名>.github.io`）的 `baseurl` 应留空。

**工作流在部署阶段失败？** 确认 **Settings → Pages → Source** 已选择 **GitHub Actions**，
而不是从分支发布。

**构建因链接检查失败？** `html-proofer` 会校验站内链接与图片。修正失效的站内链接或补上缺失的图片后重新推送。

## 许可证

见 [LICENSE](LICENSE)。
