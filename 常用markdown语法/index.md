# 常用markdown语法(hugo+loveit主题)

## 代码块
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
```bash
git clone https://github.com/dillonzq/LoveIt.git themes/LoveI
```
```scss
@import url('https://fonts.googleapis.com/css?family=Fira+Mono:400,700&display=swap&subset=latin-ext');
$code-font-family: Fira Mono, Source Code Pro, Menlo, Consolas, Monaco, monospace;
```
## 注意
{{< admonition >}}
admonition
{{< /admonition >}}

{{< admonition note "admonition note" >}}
admonition note
{{< /admonition >}}

{{< admonition tip "admonition tip">}}
admonition tip
{{< /admonition >}}
## 版本
{{< version 0.0.1 >}}

{{< version 0.0.1 changed >}}

{{< version 0.0.1 deleted >}}

