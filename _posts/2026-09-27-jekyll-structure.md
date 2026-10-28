---
title: Jekyll 目录结构与写作流程
date: 2026-09-27 20:00:00 +0800
categories: [Jekyll]
tags: [jekyll, tutorial]
description: 说明本站的目录结构、文章的文件命名规则以及从写作到发布的完整流程。
---

Jekyll 是一个把 Markdown 转换成静态站点的生成器。理解它的目录约定之后，
写作和发布都会变得非常直接。

## 目录结构

```text
.
├── _config.yml          # 站点与主题配置
├── Gemfile              # Ruby 依赖（包含 Chirpy 主题）
├── index.html           # 首页
├── _data/               # 侧边栏联系方式、分享按钮等数据
├── _plugins/            # 自定义插件（自动记录文章最后修改时间）
├── _posts/              # 文章目录
├── _tabs/               # 顶部导航（分类、标签、归档、关于）
├── assets/              # 图片、CSS 等静态资源
└── .github/workflows/   # GitHub Actions 部署工作流
```

## 文章的命名规则

文章必须放在 `_posts` 目录，文件名遵循 `YYYY-MM-DD-标题.md` 的格式：

```text
_posts/2026-09-27-jekyll-structure.md
```

其中日期会作为文章发布时间，`jekyll-structure` 则会成为文章地址的一部分：
`/posts/jekyll-structure/`。

## 文章头部信息（Front Matter）

每篇文章开头的两行 `---` 之间是 Front Matter，用来声明元数据：

```yaml
---
title: 文章标题
date: 2026-09-27 20:00:00 +0800
categories: [Jekyll]
tags: [jekyll, tutorial]
description: 一句话摘要，会用于搜索与社交分享预览。
---
```

常用的可选字段：

| 字段 | 作用 |
| --- | --- |
| `pin: true` | 把文章置顶到首页 |
| `toc: false` | 关闭该文章的目录 |
| `math: true` | 加载 MathJax，支持数学公式 |
| `mermaid: true` | 加载 Mermaid，支持流程图 |
| `image.path` | 文章封面图 |

## Liquid 模板

Jekyll 使用 Liquid 模板语言。下面的写法会输出站点标题和当前年份：

```liquid
{% raw %}{{ site.title }} · {{ site.time | date: "%Y" }}{% endraw %}
```

由于 Liquid 标签在 Markdown 中也会被执行，演示代码时需要包在 `raw` 里，
避免被提前渲染。

## 本地预览

安装 Ruby 与 Bundler 之后，在项目根目录执行：

```bash
bundle install          # 只需执行一次
bundle exec jekyll serve
```

打开 <http://127.0.0.1:4000> 即可预览。修改文件后刷新页面就能看到效果，
大部分改动不需要重启服务。

## 发布流程

1. 在 `_posts` 目录新建 Markdown 文件并编写内容
2. 本地预览确认排版正常
3. 提交并推送到 `main` 分支
4. GitHub Actions 自动构建并部署，几分钟后即可访问

> 推送之后可以在仓库的 **Actions** 页面查看构建日志，
> 构建失败时依旧保留上一次成功发布的版本。
{: .prompt-info }

完整的部署细节见 [GitHub Actions 部署指南](/posts/github-actions-deploy/)。
