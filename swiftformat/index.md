# Swiftformat

## SwiftFormat

SwiftFormat 是一个用于格式化 Swift 代码的工具。它可以帮助开发者自动整理和规范 Swift 代码的格式，使代码更加整洁和一致，从而提高代码的可读性和可维护性。SwiftFormat 可以根据一系列预定义的规则来格式化代码，开发者也可以自定义这些规则以满足特定的编码风格要求。

截止到当前,已有7.7k的star, 600+的fork, 以及 2.2k的commit, 说明这个工具还是比较受欢迎的。

地址: [SwiftFormat](https://github.com/nicklockwood/SwiftFormat)

## SwiftFormat 的主要功能

- 代码格式化：
  - 自动调整代码缩进、空格、换行等，使代码风格一致。
  - 对齐代码中的符号，如等号、冒号等。
- 代码清理：
  - 移除多余的空行、空格和注释。
  - 规范注释的格式。
- 代码重构：
  - 重新排列代码块，如方法、属性等的顺序。
  - 规范化代码中的命名规则。
- 自定义规则：
  - 支持通过配置文件定义自定义的格式化规则。
  - 可以根据团队的编码规范进行定制。
  
## 安装

安装

```shell
brew install --cask swiftformat-for-xcode
```

升级

```shell
brew upgrade --cask swiftformat-for-xcode
```

{{< admonition tip "">}}
swiftformat提供了多种安装方式,我推荐的是全局的扩展插件形式
{{< /admonition >}}

{{< image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/swiftformat_xcode_short.png" src_l="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/swiftformat_xcode_short.png" caption="Lighthouse (`image`)" width="30%">}}

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/swiftformat.png" title="" width="20%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> swiftformat </b>  </div>
</center>

## 配置快捷键

首先启用扩展插件

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/swiftformat_extension.png" title="" width="80%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> swiftformat </b>  </div>
</center>

### 配置快捷键方式1

打开Xcode -> Setting 

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/swiftformat_xcode_short.png" title="" width="80%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> Setting </b>  </div>
</center>

分割


{{< image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/swiftformat_xcode_short.png" src_l="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/swiftformat_xcode_short.png" caption="Lighthouse (`image`)" width="30%">}}

### 配置快捷键方式2

方式2参考: [链接](https://juejin.cn/post/7171725810544738317)

1.利用系统工具自动操作

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/swiftformat_auto.png" title="" width="80%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b>  </b>  </div>
</center>

2.然后打开自动操作工具进行设置

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/swiftformat_auto_setting.png" title="" width="80%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b>  </b>  </div>
</center>

所需要替换的脚本:

```shell
on run {input, parameters}
        tell application "System Events"
                tell process "Xcode"
                        set frontmost to true
                        if menu item "Format File" of menu of menu item "SwiftFormat" of menu "Editor" of menu bar 1 exists then
                                click menu item "Format File" of menu of menu item "SwiftFormat" of menu "Editor" of menu bar 1
                        end if
                        click menu item "Save" of menu "File" of menu bar 1
                end tell
        end tell
        return input
end run
```

3.给Xcode添加快捷键

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/swiftformat_short.png" title="" width="80%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> swiftformat_short </b>  </div>
</center>

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/swiftformat_xcode.png" title="" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 添加成功 </b>  </div>
</center>

4.删除脚本

```shell
cd ~/Library/Services
```

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image/blog/images/delete_swiftformat_auto.png" title="" width="30%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 删除即可 </b>  </div>
</center>
