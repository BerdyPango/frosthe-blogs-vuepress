# 技术博客攥写通用规范

本项目是一个 vuepress 驱动的博客项目，适用于所有 AI Agent（Claude Code、Codex、Cursor 等）。

---

## 文本生成/修改规范

- 始终使用简体中文
- 技术术语可保持英文原文，在中英文字符之间插入空格以保持格式美观

---

## 博客文章生成/修改规范

- 生成 Markdown 表格时，考虑格式化，以便于人类阅读

### Frontmatter 参考

- date: 应按照 `yyyy-MM-dd` 的格式来指定日期
- description: 页面的描述，它将会覆盖站点配置中的 `description` 配置项
- `excerpt`: 文章的简短摘要，作为描述文本显示在预览卡片中
- head: 类型 HeadConfig[], 页面 `<head>` 标签内添加的额外标签，示例:
```markdown
---
head:
  - - meta
    - name: foo
      content: yaml 数组语法
  - [meta, { name: bar, content: 方括号语法 }]
---
```
- layout: 页面的布局。布局是由主题提供的，如果不指定该 Frontmatter ，则会使用默认布局。
- permalink: 页面的永久链接。它将会覆盖根据文件路径来决定的默认路由路径。当被设置为 `null` 时，将会禁用页面的永久链接。
- permalinkPattern: 为页面生成永久链接的 Pattern。它将会覆盖站点配置中的 permalinkPattern 配置项。如果 Frontmatter 中设置了 permalink ，那么这个字段则不会生效。
  - `:year`: 创建日期的 `年` 部分
  - `:month`: 创建日期的 `月` 部分
  - `:day`: 创建日期的 `日` 部分
  - `:slug`: 页面文件名的 `Slug`
  - `:raw`: 原始路由路径
- routeMeta: 附加到页面路由的自定义数据。
- title: 页面的标题。如果不在 Frontmatter 中设置 title ，那么页面中第一个一级标题（即 # title）的内容会被当作标题使用。