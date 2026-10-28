---
title: Markdown 排版示例
date: 2026-09-15 21:10:00 +0800
categories: [Blogging, Markdown]
tags: [markdown, typography]
description: 汇总 Chirpy 主题支持的各种 Markdown 写法，包括标题、列表、表格、代码块与提示框。
---

本文汇总本站常用的 Markdown 写法，方便写作时随时查阅。

## 标题与正文

### 三级标题

正文支持 **加粗**、_斜体_、~~删除线~~、`行内代码` 以及 [链接](/posts/hello-chirpy/)。
中英文混排时，Chirpy 会自动为正文选择合适的字体。

### 列表

无序列表：

- 第一项
- 第二项
  - 嵌套的子项
  - 另一个子项
- 第三项

有序列表：

1. 安装依赖
2. 本地预览
3. 推送部署

任务清单：

- [x] 选定主题
- [x] 配置站点信息
- [ ] 补充更多文章
- [ ] 添加评论系统

### 引用

> 把复杂的东西讲简单，把简单的东西讲准确。
>
> —— 某位工程师

## 表格

| 语法 | 效果 | 备注 |
| --- | --- | :---: |
| `**粗体**` | **粗体** | 常用 |
| `*斜体*` | _斜体_ | 常用 |
| `~~删除~~` | ~~删除~~ | GFM 扩展 |
| `` `代码` `` | `代码` | 行内 |

## 代码块

代码块会自动高亮，并支持一键复制。

```yaml
# _config.yml 片段
title: Guxin
lang: zh-CN
timezone: Asia/Shanghai
url: "https://guxin0123.github.io"
```

```javascript
function greet(name) {
  console.log(`你好，${name}！`);
}

greet("Chirpy");
```

```bash
bundle install
bundle exec jekyll serve
```

带行号与高亮标记的代码块：

```ruby
def deploy(env = :production)
  puts "Building for #{env}"
  system("bundle exec jekyll build")
end
```

### 提示框

> 这是一条普通提示，适合补充说明。
{: .prompt-tip }

> 这是一条信息提示，适合给出背景知识。
{: .prompt-info }

> 注意：执行删除操作前请先备份数据。
{: .prompt-warning }

> 危险操作：请不要在公开分支上提交密钥。
{: .prompt-danger }

## 图片与文件路径

![示例地址](/assets/img/sample-banner.png)
_图片标题会显示在图片下方。_
{: .text-center }

配置文件位于 `_config.yml`{: .filepath }，文章放在 `_posts` 目录中。

## 脚注与分隔线

这里引用一个脚注[^1]。

[^1]: 脚注的内容会显示在文章末尾。

---

以上覆盖了日常写作所需的大部分语法，更多用法可以参考
[Chirpy 官方文档](https://github.com/cotes2020/jekyll-theme-chirpy/wiki)。
