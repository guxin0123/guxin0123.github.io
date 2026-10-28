---
title: GitHub Actions 部署指南
date: 2026-10-12 19:45:00 +0800
categories: [DevOps]
tags: [github-actions, jekyll, deployment]
description: 介绍本站如何通过 GitHub Actions 构建并发布到 GitHub Pages，以及常见问题的排查方法。
---

本站没有使用 GitHub Pages 的「分支构建」，而是通过 **GitHub Actions** 自行安装依赖、
构建站点并上传产物。原因很简单：Chirpy 是一个 RubyGem 主题，自带插件与构建流程，
只有自行运行 `bundle exec jekyll build` 才能完整地使用它的功能。

## 一次性配置

推送代码之前，需要先把 Pages 的构建来源切换到 Actions：

1. 打开仓库的 **Settings → Pages**
2. 在 **Build and deployment → Source** 中选择 **GitHub Actions**
3. 推送任意提交，触发 `Build and Deploy` 工作流

> 如果 Source 仍然停留在「Deploy from a branch」，工作流会在部署阶段失败，
> 因为仓库中并没有可以直接发布的 `_site` 目录。
{: .prompt-warning }

## 工作流做了什么

工作流文件位于 `.github/workflows/pages-deploy.yml`，核心步骤只有四个：

{% raw %}
```yaml
- name: Setup Ruby
  uses: ruby/setup-ruby@v1
  with:
    ruby-version: 3.4
    bundler-cache: true

- name: Build site
  run: bundle exec jekyll b -d "_site${{ steps.pages.outputs.base_path }}"
  env:
    JEKYLL_ENV: "production"

- name: Test site
  run: bundle exec htmlproofer _site --disable-external

- name: Upload site artifact
  uses: actions/upload-pages-artifact@v5
  with:
    path: "_site${{ steps.pages.outputs.base_path }}"
```
{% endraw %}

- **Setup Ruby**：按 `Gemfile` 安装主题依赖，`bundler-cache` 会缓存 gem，后续构建会快很多
- **Build site**：以生产模式构建，输出到 `_site`
- **Test site**：用 `html-proofer` 检查站内链接与图片，发现问题会中断部署
- **Upload / Deploy**：把构建产物交给 Pages 发布

## 关键配置

`_config.yml` 中的两个地址必须和实际部署位置一致：

```yaml
url: "https://guxin0123.github.io"
baseurl: ""
```

`guxin0123.github.io` 属于**用户站点**，域名本身没有子路径，所以 `baseurl` 留空即可。
如果仓库名不是 `<用户名>.github.io`，则是项目站点，此时需要写成：

```yaml
url: "https://guxin0123.github.io"
baseurl: "/repository-name"
```

## 常见问题

### 构建成功但页面样式错乱

通常是 `url` 或 `baseurl` 写错导致 CSS 与 JS 的路径不对。检查浏览器控制台的 404 请求，
确认资源地址是否带上了正确的前缀。

### 部署失败并提示 Pages 未启用

确认 **Settings → Pages → Source** 已经选择 **GitHub Actions**，
并且工作流的 `permissions` 中包含 `pages: write` 与 `id-token: write`。

### 想手动重新发布

在仓库的 **Actions** 页面选择 `Build and Deploy`，点击 **Run workflow** 即可手动触发。

## 小结

整套流程不需要服务器，也不需要提交构建产物。写文章时只需要专注 Markdown，
推送之后剩下的交给 Actions 完成。
