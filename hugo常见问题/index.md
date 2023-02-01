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
代码块识别语言高亮,配置文件路径:
```bash
/themes/LoveIt/assets/css/_variables.scss
```
### 链接
#### 链接 - 普通链接
```markdown
{{</* link "https://andy90s.github.io/" */>}}
或者
{{</* link href="https://andy90s.github.io/" content="https://andy90s.github.io/" */>}}

{{</* link "mailto:contact@qq.com" */>}}
或者
{{</* link href="mailto:contact@qq.com" content="mailto:contact@qq.com" */>}}

{{</* link "https://andy90s.github.io/" Andy90s */>}}
或者
{{</* link href="https://andy90s.github.io/" content=Andy90s */>}}

<!-- 停留带标题 -->
{{</* link "https://andy90s.github.io/" Andy90s "Visit Andy90s!" */>}}
或者
{{</* link href="https://andy90s.github.io/" content=Andy90s title="Visit Andy90s!" */>}}
```
效果如下:

{{< link "https://andy90s.github.io/" >}}   
    或者    
{{< link href="https://andy90s.github.io/" content="https://andy90s.github.io/" >}}    

{{< link "mailto:contact@qq.com" >}}   
    或者    
{{< link href="mailto:contact@qq.com" content="mailto:contact@qq.com">}}   

{{< link "https://andy90s.github.io/" Andy90s >}}   
    或者    
{{< link href="https://andy90s.github.io/" content=Andy90s >}}        

<!-- 停留带标题 -->
{{< link "https://andy90s.github.io/" Andy90s "Visit Andy90s!" >}}    
或者    
{{< link href="https://andy90s.github.io/" content=Andy90s title="Visit Andy90s!" >}}   

#### 链接 - 内部跳转链接

```markdown
<!-- ref绝对路径 relref相对路径 -->
{{</* ref "path/to/document.md#锚点" */>}} 
{{</* relref "path/to/document.md#锚点" */>}}
```

例如跳转到本文的`文章目录`锚点:
```markdown
[跳转到其他文章锚点]({{</* relref "其他文章.md#文章目录" */>}})
[跳转到文章目录]({{</* relref "../other/hugo常见问题.md#文章目录" */>}})
```
效果:
[跳转到文章目录]({{<relref "../other/hugo常见问题.md#文章目录">}})

## gitalk评论设置
### `github`生成application auth 
生成方法:打开github设置 - develop setting - OAuth Apps - 选择新建

<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/271669958429_.pic.jpg" title="新建OAuth" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 新建OAuth </b>  </div>
</center>

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

## 参考
[主题文档 - 扩展 Shortcodes](https://hugoloveit.com/zh-cn/theme-documentation-extended-shortcodes/#2-link)
