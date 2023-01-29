# Xcode相关问题记录

<!--more-->
## 前言
本文主要记录Xcode使用遇到的一些问题,以及当时版本的解决方案.
## Xcode下载
主要有两种途径:
### 方式1:  App Store
苹果商店搜索`Xcode`即可下载.    
优点: 可自动更新.    
缺点: 安装的时候经常会遇到最后卡住,或者安装不上等问题.
### 方式2:  官网下载Xip包
[下载地址](https://developer.apple.com/download/all/?q=for%20Xcode),登录苹果账号进行下载.      
优点: 弥补了方式1的缺点,并且历史版本也可以下载安装.     
缺点: 无法自动更新下一个版本.
## 重装Xcode之后,Cocoapods报错问题
### 如下报错:
```
/.rvm/rubies/ruby-3.0.0/lib/ruby/gems/3.0.0/gems/cocoapods-1.11.3/lib/cocoapods/command.rb:128:in `git_version': Failed to extract git version from `git --version` ("xcrun: error: active developer path (\\"/Users/wangzhu/Downloads/Xcode.app/Contents/Developer\\") does not exist\\nUse `sudo xcode-select --switch path/to/Xcode.app` to specify the Xcode that you wish to use for command line developer tools, or use `xcode-select --install` to install the standalone command line developer tools.\\nSee `man xcode-select` for more details.\\n") (RuntimeError)
from /Users/wangzhu/.rvm/rubies/ruby-3.0.0/lib/ruby/gems/3.0.0/gems/cocoapods-1.11.3/lib/cocoapods/command.rb:140:in `verify_minimum_git_version!'
from /Users/wangzhu/.rvm/rubies/ruby-3.0.0/lib/ruby/gems/3.0.0/gems/cocoapods-1.11.3/lib/cocoapods/command.rb:49:in `run'
from /Users/wangzhu/.rvm/rubies/ruby-3.0.0/lib/ruby/gems/3.0.0/gems/cocoapods-1.11.3/bin/pod:55:in `<top (required)>'
from /usr/local/bin/pod:25:in `load'
from /usr/local/bin/pod:25:in `<main>'
from /Users/wangzhu/.rvm/rubies/ruby-3.0.0/bin/ruby_executable_hooks:22:in `eval'
from /Users/wangzhu/.rvm/rubies/ruby-3.0.0/bin/ruby_executable_hooks:22:in `<main>'
```
### 解决办法:    
#### 1.终端执行下面命令  
```
xcode-select --install
```
admoni

#### 2.然后尝试执行
```
pod --version
```
如果正常显示版本号,命令行工具已正常工作,到此解决.如果还是报错看步骤3.    
#### 3.到此如果cocoapods还是无法正常使用,执行上面步骤1提示如下:
```
xcode-select: error: command line tools are already installed, use "Software Update" in System Settings to install updates
```
按照提示更新下工具:   
```
softwareupdate --install -a
```
执行步骤2查看版本号正常显示版本,到此解决.


