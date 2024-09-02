# SwiftLint

<!--more-->

## SwiftLint

`SwiftLint`是一个用于强制执行Swift代码样式和规则的工具。它基于Swift语言编写，可以在构建时或作为一个独立的工具运行。它可以帮助您避免在代码审查时讨论样式问题，并确保您的代码符合Swift样式指南。

目前已经18.5k的star了,好不好用已经说明了一切.

## 安装

### Homebrew 全局安装当前电脑环境

```shell
brew install swiftlint
```

### CocoaPods 集成到项目中

```shell
pod 'SwiftLint'
```

{{< admonition tip "">}}
两种方式都可以, 比如单人项目可以使用Homebrew全局安装, 多人项目可以使用CocoaPods集成到项目中.
{{< /admonition >}}

## 配置

首先在项目根目录下创建一个`.swiftlint.yml`文件，然后在文件中添加配置项。

```yaml
excluded:
  - Pods
disabled_rules:
    - trailing_whitespace # 每一个空行不能有空格，会与Xcode换行后自动对齐生成的空格冲突，建议排除掉加。
    - missing_docs # 缺失说明注释, 官方解释：”Public declarations should be documented.”， 公共声明应该被注释/标记。 在函数声明的时候， 一般情况下， 带public关键字的函数的注释只能用 “///”和 “/* /”来注释， 如果不带public关键字的函数只能用 “//”和 “/* */” 。这个属性应该禁用，没必要！！！
    - function_parameter_count # 函数参数计数违例:函数应该有5个参数，多余会报错 函数参数个数， 函数参数数量(init方法除外)应该少点， 不要太多，swiftlint规定函数参数数量超过5个给warning， 超过8个直接报error。这个属性推荐使用， 由于简单就不举例了。注：function_parameter_count: error 这样并不能改变它的警告或错误，该属性不允许修改，但是可以禁用
    - identifier_name   #命名规则必须按照驼峰原则（可能model中的某些字段与json字段命名冲突，建议排除掉）
    - type_name #类型命名规则限制,以大写字母开头，且长度在1到20个字符之间
    - shorthand_operator #使用+= ， -=， *=， /=  代替 a = a + 1
    - large_tuple #元祖成员 元组冲突:元组应该最多有2个成员，多余两个会报错
    - for_where #使用 `for where` 代替 简单的 `for { if }`
    - class_delegate_protocol #delegate protocol 应该被设定为 class-only,才能被弱引用
    - todo #避免 TODOs and FIXMEs 标识
cyclomatic_complexity: 20 #代码复杂度,默认为10
force_try: warning # try语句判断
force_cast: warning # 强制转换（代码中存在一些前面通过if判断过类型，后面做的强制转换的代码）
line_length:    #每行长度限制
  warning: 160
  error: 300
  ignores_function_declarations: true
  ignores_comments: true
file_length:    #文件长度
  warning: 1000
  error: 1500
function_body_length:   #函数体长度
  warning: 100
  error: 150
type_body_length:   #类的长度
  warning: 800
  error: 1200
```

## 全局方式脚本配置

点击目标target  -> Build Phases -> 点击左上角的+号 -> New Run Script Phase

intel芯片:

```shell
if command -v swiftlint >/dev/null 2>&1
then
    swiftlint
else
    echo "warning: `swiftlint` command not found - See https://github.com/realm/SwiftLint#installation for installation instructions."
fi
```

M1芯片:(如果你报错path not found,可以尝试这个)

```shell
if [[ "$(uname -m)" == arm64 ]]
then
    export PATH="/opt/homebrew/bin:$PATH"
fi

if command -v swiftlint >/dev/null 2>&1
then
    swiftlint
else
    echo "warning: `swiftlint` command not found - See https://github.com/realm/SwiftLint#installation for installation instructions."
fi
```
## Cocoapods方式脚本配置方式

如果使用的cocoapods的方式,在工程中需要配置脚本:

```shell
"${PODS_ROOT}/SwiftLint/swiftlint"
```

## 使用

配置好脚本之后,直接build项目即可.

{{< admonition tip "">}}
建议搭配`swiftformat`使用, 两者结合使用效果更香.
{{< /admonition >}}




