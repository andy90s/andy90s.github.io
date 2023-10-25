# Fastlane打包

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
工程自动签名:
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
工程手动签名:
```
ENV["FASTLANE_XCODEBUILD_SETTINGS_TIMEOUT"] = "120" # 设置超时时间,不然会报超时错误
default_platform(:ios)
platform :ios do
  desc "Description of what the lane does"

  lane :beta do
	
	increment_build_number
	gym(
		output_directory: "./fastlane/", # 打包后的ipa文件存放的路径
		export_method: "ad-hoc",
		scheme: "XXX", # 项目的scheme
		export_options: {
			provisioningProfiles: {
				"包名" => "描述文件名称"
  			}
		},
		configuration: "Release"
	)
	pgyer(api_key: "蒲公英的api_key")
  end
end

```
打包的时候到项目根目录执行:
```
fastlane beta
```

## 二次签名

准备好需要二次签名的ipa文件,然后执行以下命令:
```
fastlane sigh resign --signing_identity "iPhone Distribution: XXXX" --provisioning_profile "描述文件名称.mobileprovision" --ipa "需要二次签名的ipa文件路径" --entitlements "entitlements.plist" --display_name "显示名称" --bundle_id "包名" --output_path "输出路径"
```
其中`signing_identity`是证书的名称,可以在钥匙串中查看,`provisioning_profile`是描述文件的名称,可以在`~/Library/MobileDevice/Provisioning Profiles`目录下查看,`ipa`是需要二次签名的ipa文件路径,`entitlements`是entitlements文件的路径,`display_name`是显示名称,`bundle_id`是包名,`output_path`是输出路径
