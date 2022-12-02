# Hugo常见问题


## 文章通用配置
打开`archetypes/default.md`文件进行编辑:
```toml
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: false 
categories: ["Web"]
tags: ["HTML"]
keywords: ["HTML"]
---
```
{{< admonition tip "关键字说明">}}
`draft`: 是否为草稿,当为`true`的时候,发布到网站是不显示的.
{{< /admonition >}}
