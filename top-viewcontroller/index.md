# 获取当前显示的ViewController

<!--more-->

## 前言
很多场景需要获取当前显示的控制器，比如弹窗，需要获取当前显示的控制器，才能弹出来，否则会出现问题。这里记录一下获取当前显示的控制器的方法。

## 获取当前显示的控制器
### 首先获取当前显示的window

```swift
extension UIApplication {
    /// 获取keywindow
    static var currentKeyWindow: UIWindow? {
        if #available(iOS 13.0, *) {
            return UIApplication.shared.connectedScenes
                .filter {$0.activationState == .foregroundActive}
                .compactMap {$0 as? UIWindowScene}
                .first?.windows
                .filter {$0.isKeyWindow}.first
        } else {
            return UIApplication.shared.keyWindow
        }
    }
}
```
{{< admonition tip "">}}
关于`keyWindow`可以看这里 [获取keyWindow]({{<relref "./keywindow.md">}})
{{< /admonition >}}
### 获取当前显示的控制器
```swift
extension UIApplication {
    /// 获取当前显示的控制器
    /// - Parameter base: 基础控制器
    /// - Returns: 当前显示的控制器
    class func topViewController(base: UIViewController? = UIApplication.currentKeyWindow?.rootViewController) -> UIViewController? {
        if let nav = base as? UINavigationController {
            return topViewController(base: nav.visibleViewController)
        }
        if let tab = base as? UITabBarController {
            if let selected = tab.selectedViewController {
                return topViewController(base: selected)
            }
        }
        if let presented = base?.presentedViewController {
            return topViewController(base: presented)
        }
        return base
    }
    class var topViewController: UIViewController? {
        return topViewController()
    }
}
```
{{< admonition tip "建议">}}
建议不要在`viewDidLoad`中使用`topViewController`，因为此时`view`还没有加载完成，会导致获取到的控制器不正确。
{{< /admonition >}}

