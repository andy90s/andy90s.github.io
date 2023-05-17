# 退到后台通知引起的崩溃

<!--more-->

```swift
@objc func appDidEnterBackground() {
    // 要判断当前应用程序状态是否为后台状态
    guard UIApplication.shared.applicationState == .background else { return }
    // todo something
}
```
