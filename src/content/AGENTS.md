# Firefly 博客文章编写指南

## 文章目录

文章文件放置在 `src/content/posts/` 目录中。支持直接放置 `.md` / `.mdx` 文件，也可以创建子目录来组织文章和资源。

```
src/content/posts/
├── assets
├── post-1.md
├── post-2.md
└── category/
    ├── assets
    └── post-3.md
    └── sub-category/
        ├── assets
        └── post-4.md
```

## 文章编写规范

详细规范请参考：[Firefly 编写文章指南](https://docs-firefly.cuteleaf.cn/zh/guide/writing.html)

### Front-matter 必填字段

每篇文章顶部使用 YAML 格式的 Front-matter 来定义元数据：

```yaml
---
title: 文章标题          # 必填
published: 2025-01-01   # 必填，发布日期
description: 文章描述    # 可选
image: ./cover.jpg      # 可选，封面图片；默认使用文章中第一张图片，如果没有则使用`api`随机封面图
tags: [标签1, 标签2]     # 可选
category: 分类          # 可选
draft: false            # 可选，是否为草稿
---
```
| 属性           | 类型      | 必填 | 说明                             |
| -------------- | --------- | ---- |--------------------------------|
| `title`        | `string`  | 是   | 文章标题                           |
| `published`    | `date`    | 是   | 发布日期                           |
| `updated`      | `date`    | 否   | 更新日期，未设置则默认使用发布日期              |
| `description`  | `string`  | 否   | 文章简短描述，显示在首页文章卡片上              |
| `image`        | `string`  | 否   | 封面图片路径，`api`:随机封面图                  |
| `tags`         | `string[]`| 否   | 文章标签                           |
| `category`     | `string`  | 否   | 文章分类                           |
| `draft`        | `boolean` | 否   | 是否为草稿，草稿不会对读者可见                |
| `pinned`       | `boolean` | 否   | 是否置顶在文章列表顶部                    |
| `slug`         | `string`  | 否   | 自定义 URL 路径                     |
| `lang`         | `string`  | 否   | 文章语言代码（如 zh-CN），仅当与站点默认语言不同时设置 |
| `author`       | `string`  | 否   | 文章作者                           |
| `comment`      | `boolean` | 否   | 是否启用评论，默认 true                 |
| `licenseName`  | `string`  | 否   | 自定义许可证名称                       |
| `licenseUrl`   | `string`  | 否   | 自定义许可证链接                       |
| `sourceLink`   | `string`  | 否   | 文章来源链接                         |
| `password`     | `string`  | 否   | 文章密码，设置后文章将被加密保护，详见 文章加密       |
| `passwordHint` | `string`  | 否   | 密码提示，显示在密码输入框上方                |

### 支持的功能

- **数学公式 (KaTeX)**：行内公式 `$...$`，块级公式 `$$...$$`
- **Mermaid 图表**：流程图、时序图、甘特图等
- **提醒框**：NOTE、TIP、IMPORTANT、WARNING、CAUTION
- **GitHub 仓库卡片**：`::github{repo="owner/repo"}`
- **嵌入视频**：支持 YouTube、Bilibili 等
- **剧透文本**：`:spoiler[隐藏内容]`

### 文件格式

- 推荐使用 `.md` 格式
- 需要嵌入 JSX 组件或动态数据时使用 `.mdx` 格式