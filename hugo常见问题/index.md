# Hugo常见问题

<!--more-->
## 文章目录
```toml
# 目录设置
  [markup.tableOfContents]
    startLevel = 2 # 代表从第几级标题开始生成目录
    endLevel = 6 # 代表从第几级标题结束目录
    ordered = true # 目录排序
```

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
## 常用指令
```toml
hugo new posts 文章.md # 发布文章
hugo # 编译
hugo server -D # 启动本地服务
```

## loveit主题相关


## gitalk评论设置
### `github`生成application auth 
生成方法:打开github设置 - develop setting - OAuth Apps - 选择新建
![新建OAuth](https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/271669958429_.pic.jpg "新建OAuth")

### 得到秘钥,粘贴到配置中
```toml
[params.page.comment.gitalk]
        enable = true
        owner = "andy90s"
        repo = "andy90s.github.io"
        clientId = "上面拿到的id"
        clientSecret = "上面生成的秘钥"
        id = "location.pathname"
```
{{< admonition note "注意">}}
上述评论配置为(`hugo+loveit主题`),其他配置差别不大,注意`repo`应填写`仓库名`即可     
`id`按照上述配置,会自动生成`issue`
{{< /admonition >}}

