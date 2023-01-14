# 常用markdown语法(hugo)

<!--more-->
## 前言
本文大部分是在`hugo`+`loveit`主题两者基础上才有作用,具体参考loveit主题[shortcode文档](https://hugoloveit.com/zh-cn/theme-documentation-extended-shortcodes/#1-style)
## 代码块
### TOML
```toml
baseURL = "http://example.org/"

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"

# 网站标题
title = "我的全新 Hugo 网站"

# 网站语言, 仅在这里 CN 大写 ["en", "zh-CN", "fr", "pl", ...]
languageCode = "zh-CN"
# 语言名称 ["English", "简体中文", "Français", "Polski", ...]
languageName = "简体中文"
# 是否包括中日韩文字
hasCJKLanguage = true
```
### bash
```bash
git clone https://github.com/dillonzq/LoveIt.git themes/LoveI
```

### css
```scss                
@import url('https://fonts.googleapis.com/css?family=Fira+Mono:400,700&display=swap&subset=latin-ext');             
$code-font-family: Fira Mono, Source Code Pro, Menlo, Consolas, Monaco, monospace;           
```     

## admonition
{{< admonition >}}
注意
{{< /admonition >}}

{{< admonition note "admonition note" >}}
admonition note
{{< /admonition >}}

{{< admonition tip "admonition tip">}}
admonition tip
{{< /admonition >}}
{{<link href="https://hugoloveit.com/zh-cn/theme-documentation-extended-shortcodes/#4-admonition" content="【更多admonition参考】">}}
## 版本
{{< version 0.0.1 >}}       
{{< version 0.0.2 changed >}}       
{{< version 0.0.3 deleted >}}           
## 图片
### 默认
```scss
![](/images/fengmian4.jpg)
```
![](/images/fengmian4.jpg)
### 控制大小和位置(1)
```scss
<p align="center">
    <img src="/images/fengmian4.jpg" width="200" />
</p>
<center>图示</center>

```
<p align="center">
    <img src="/images/fengmian4.jpg" width="300" />
</p>
<center>图示</center>

### 控制大小和位置(2)
```scss
<center>
<img width="300" src="/images/fengmian4.jpg">
<div style="color:black;"> <b> 图示 </b>  </div>
</center>
```
<center>
<img width="300" src="/images/fengmian4.jpg">
<div style="color:black;"> <b> 图示 </b>  </div>
</center>

### 控制大小和位置(3)
```scss
<center>
{{</* image src="/images/fengmian4.jpg" title="停留显示"width="50%" */>}}
<div style="color:black;"> <b> 图示 </b>  </div>
</center>
```
{{< admonition tip>}}
1.此shortcode需`loveit`主题支持            
2.这种方式显示的图片配合loveit主题可以点击放大,停留显示文案等
{{< /admonition >}}

<center>
{{<image src="/images/fengmian4.jpg" src_s="/images/fengmian4.jpg" src_l="/images/fengmian4.jpg" title="停留显示"width="50%">}}
<div style="color:black;"> <b> 图示 </b>  </div>
</center>

{{< admonition bug>}}
主题应该是有bug, 当设置`caption`图片标题, 会造成`width`属性失效,所以这里加`div`标签达到效果     
{{< /admonition >}}
