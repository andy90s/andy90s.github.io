# UIDevice

<!--more-->
## 前言
关于设备信息,苹果提供的API并不多,但是我们可以通过一些开源库来获取更多的信息,比如设备型号,屏幕尺寸,系统版本等等.比如获取当前设备是否是刘海屏,如果用安全距离来判断,因为window有可能是nil,可能会导致判断错误
## DeviceKit
### 地址
{{<link href="https://github.com/devicekit/DeviceKit" content="【DeviceKit】">}}
### 使用
比如判断是否是刘海屏等:<br>

```swift
import DeviceKit

let device = Device.current
// 判断是否是iphone
if device.isPhone {
    // 判断是否是刘海屏
    if device.hasRoundedDisplayCorners {
        // 判断是否是iphoneX
        if device == .iPhoneX {
            // ...
        }
    }
}
```

iOS14前，iPhone状态栏高度只有两种，刘海屏44pt，非刘海屏20pt。

iOS14后，刘海屏的状态栏高度不再是固定的44pt。

手机机型	状态栏高度	顶部安全区域
iPhone 15 Pro Max/15 Pro/15 Plus/15/14 Pro Max/14 Pro	54	59
iPhone 14/13 Pro/13/12 Pro/12	47	47
iPhone 13 mini/12 mini	50	50
iPhone 11/XR	48	48
其他刘海屏	44	44
非刘海屏	20	20


