# Telegram-iOS编译打包

<!--more-->
## 前言
Telegram开源IM应用,虽然服务器代码不开源,但是可以从客户端的体验来看非常流畅,故下载编译学习记录.

## 创建应用(只编译Xcode工程此步骤不需要看)
1. 打开[官网](https://my.telegram.org/),选择**API development tools**

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/202301281427085.png" title="选择API development tools" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> API development tools </b>  </div>
</center>

2. 填写自己的电报号码登录,验证码为官方TG给你发条验证消息:
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/202301281435414.png" title="TG账号登录" width="70%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> TG账号登录 </b>  </div>
</center>

3. 选择**API**,**Getting started**下面的**Creating an application**
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/202301281451551.png" title="Creating an application" width="60%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> Creating an application </b>  </div>
</center>

4. 然后在表单页面填写应用信息,如App名称等,最后提交即可.
{{< admonition tip "注意">}}
如果提交的时候出现报错`Error`,需要科学上网(全局模式).
{{< /admonition >}}

5. 生成的应用示例:
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/202301281506469.png" title="示例" width="80%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 示例 </b>  </div>
</center>

## 源码下载
[iOS地址](https://github.com/TelegramMessenger/Telegram-iOS)    
```bash
git clone --recursive -j8 https://github.com/TelegramMessenger/Telegram-iOS.git
```
{{< admonition tip "提醒">}}
此处加参数是为了拉取子模块,子模块要拉取完整后续才能编译成功.
{{< /admonition >}}

## 环境设置
打开下载的项目文件夹,找到`versions.json`文件,并打开:
```toml
{
    "app": "9.3",
    "bazel": "5.3.1",
    "xcode": "14.1"
}

```
确认到环境为Xcode为14.1版本,bazel要求5.3.1

由于我已经安装Xcode14.2版本,所以这里编辑Xcode版本号为当前电脑已安装Xcode版本.

安装bazel:
```bash
brew install bazel XXX(版本号)
```
如果提示没有版本号,可以去[官网](https://github.com/bazelbuild/bazel/releases?q=5.3.1&expanded=true)直接看文档下载安装.

如果你是第一次安装bazel到此环境问题就解决了,安装过的看下面👇🏻    
由于bazel我是之前已经安装过(通过homebrew安装),默认为最新版本,需要切换为配置文件版本,不然后续编译会报错

```bash
brew tap bazelbuild/tap
brew extract bazel bazelbuild/tap --version 5.3.2
brew install bazel@5.3.2
```
{{< admonition tip "注意">}}
当我执行如上发现**brew**并没有5.3.1版本,找到[历史版本](https://github.com/Homebrew/homebrew-core/commits/master/Formula/bazel.rb),发现比较接近的有5.3.2,将版本号修改并执行指令(`versions.json`文件中也修改一致,但是bazel版本不能和官方相差太大,会报错).
如果和我一样之前已经安装了新版本,需要链接到下载的旧版本:
```bash
brew install bazelbuild/tap/bazel@5.3.2
brew link bazel@5.3.2
```
{{< /admonition >}}



## 编译Xcode工程

1. **复制配置到电脑根目录**
```bash
mkdir -p $HOME/telegram-configuration
cp -R build-system/example-configuration/* $HOME/telegram-configuration/
cp -R build-system/appstore-configuration.json $HOME/telegram-configuration/
cp -R build-system/fake-codesigning/profiles $HOME/telegram-configuration/provisioning/
```
2. **创建缓存文件夹(可选)**
```bash
mkdir -p "$HOME/telegram-bazel-cache"
```
最终我的电脑根目录下结构如图所示:
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/202301291639284.png" title="目录结构" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 目录结构 </b>  </div>
</center>

3. **脚本生成**
```bash
python3 build-system/Make/Make.py \
    --cacheDir="$HOME/telegram-bazel-cache" \
    generateProject \
    --configurationPath="$HOME/telegram-configuration/appstore-configuration.json" \
    --disableExtensions \
    --codesigningInformationPath="$HOME/telegram-configuration/provisioning"
```
python3 build-system/Make/Make.py \
    --cacheDir="$HOME/telegram-bazel-cache" \
    generateProject \
    --configurationPath="$HOME/telegram-configuration/appstore-configuration.json" \
    --codesigningInformationPath="$HOME/telegram-configuration/provisioning"

到此Xcode工程成功生成,友情提示代码注释几乎没有...
## 真机运行问题
打开app,白屏或者黑屏,原因是app-group 没有添加,在Xcode工程配置上添加自己设置好的即可.
## 打包
打开`HOME/telegram-configuration/`路径下的**variables.bzl**(就是上一步复制到电脑根目录的配置文件的路径),原配置如下:
```toml
telegram_bundle_id = "ph.telegra.Telegraph"
telegram_api_id = "8"
telegram_api_hash = "7245de8e747a0d6fbe11f7cc14fcc0bb"
telegram_team_id = "C67CF9S4VU"
telegram_app_center_id = "0"
telegram_is_internal_build = "false"
telegram_is_appstore_build = "true"
telegram_appstore_id = "686449807"
telegram_app_specific_url_scheme = "tg"
telegram_premium_iap_product_id = "org.telegram.telegramPremium.monthly"
telegram_aps_environment = "production"
telegram_enable_siri = True
telegram_enable_icloud = True
telegram_enable_watch = True
```
待施工. 还没有进行打包.


