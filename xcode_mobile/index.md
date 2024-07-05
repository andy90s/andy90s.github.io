# Xcode运行真机卡住,Xcode崩溃

## 环境

{{< admonition note "当前环境">}}
- Xcode Version 15.4 (15F31d)
- MacOS 14.5 (23F79)
- iPhone XS iOS 17.5.1 (21F90)
{{< /admonition >}}

## 问题

{{< admonition bug "">}}
同一个设备,在Xcode中显示两个,名字相同

点击任意一个运行真机,卡住,然后Xcode崩溃
{{< /admonition >}}



## 解决

1. 打开Divce Support文件夹,删除对应设备的文件夹

{{< admonition tip "">}}
Finder -> 前往(command + shift + G)-> 输入路径
```bash
~/Library/Developer/Xcode/iOS DeviceSupport
```
{{< /admonition >}}

2. 删除对应的设备,然后手机重新连接电脑,重新信任匹配设备

{{< admonition tip "">}}
打开Xcode -> Window -> Devices and Simulators -> 选中对应设备 -> unpair
{{< /admonition >}}







