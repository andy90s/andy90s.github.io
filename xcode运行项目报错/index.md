# Xcode运行项目报错

<!--more-->

## M系列电脑运行`MAC(Designed for iPad)`或者`MAC(Designed for iPhone)`项目报错
### 报错信息
```error
The app's bundle identifier "com.xxx.xxx.xxx" is being used by another running app. 
macOS does not support running iOS or Mac Catalyst apps concurrently with other apps using the same bundle identifier. 
Please try again after terminating all other processes using the same bundle identifier or changing the bundle identifier of your app.
```
### 分析原因
- 电脑上有同名应用
### 解决方案
- 检查当前电脑应用是否有同名应用，如果有，关闭同名应用
- 重启Xcode
- 上面不行直接施展重启大法: **重启电脑**
   
