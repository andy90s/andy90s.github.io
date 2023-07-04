# Fastlane自动打包

<!--more-->
## fastlane简介
`fastlane`是一个用来自动化打包和发布的工具,可以用来自动化打包,上传到`App Store`或者`TestFlight`,也可以用来自动化发布到`fir`或者`蒲公英`等平台,还可以用来自动化生成`icon`和`splash`等等,总之,`fastlane`是一个非常强大的工具,可以大大提高开发效率,减少出错的概率,下面就来介绍一下`fastlane`的使用
## 使用
### 安装
使用brew安装
```
brew install fastlane
```
### 初始化
在项目根目录下执行
```
fastlane init
```
然后选择`2`来初始化`fastlane`配置文件,这样就会在项目根目录下生成一个`fastlane`文件夹,里面有一个`Fastfile`文件,这个文件就是`fastlane`的配置文件,我们可以在这个文件中配置自动化打包的一些参数,比如`bundleId`等等,也可以在这个文件中配置自动化上传到`App Store`或者`TestFlight`等等,具体的配置可以参考[fastlane官方文档](https://docs.fastlane.tools/)

## 以下是我自己上传到蒲公英的脚本
```
ENV["FASTLANE_XCODEBUILD_SETTINGS_TIMEOUT"] = "120" # 设置超时时间,不然会报超时错误 
default_platform(:ios)

platform :ios do
  desc "Description of what the lane does"
  lane :beta do
	gym(
	 export_method: "ad-hoc",
	 scheme: "XXX", # 项目的scheme
		configuration: "Release"
	)
	pgyer(api_key: "xxx") # 蒲公英的api_key,可到蒲公英网站查看
  end
end
```
打包的时候到项目根目录执行:
```
fastlane beta
```
