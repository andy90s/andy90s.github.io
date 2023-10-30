# keyWindow

<!--more-->
## 前言
`keyWindow`是iOS系统中的一个概念,它是一个全局的变量,代表当前显示在屏幕最上面的window,它是一个`UIWindow`对象,每个应用程序都有一个`keyWindow`,它是一个全局的变量,可以通过`UIApplication`的`keyWindow`属性获取,但是由于
iOS系统的不断迭代,导致获取keywindow的方法也在不断变化,这里记录一下获取keywindow的方法

## 获取keywindow
### iOS13之前
```swift
UIApplication.shared.keyWindow
```
### iOS13之后
```swift
UIApplication.shared.windows.filter {$0.isKeyWindow}.first
```
### iOS16之后
```swift
UIApplication.shared.connectedScenes
    .filter {$0.activationState == .foregroundActive}
    .map {$0 as? UIWindowScene}
    .compactMap {$0}
    .first?.windows
    .filter {$0.isKeyWindow}.first
```
{{< admonition tip "">}}
获取逻辑: 多个场景 -> 筛选出前台激活的场景 -> 转换为`UIWindowScene` -> 去除nil -> 取第一个场景 -> 获取场景中的窗口 -> 筛选出keywindow -> 取第一个窗口
{{< /admonition >}}
### 适配所有版本
由于新的api已经兼容到了13.0,所以只需要兼容到13.0就可以了,这里使用`#available`来判断版本,如果版本大于13.0,则使用新的api,否则使用旧的api
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
{{< admonition tip "注意">}}
需要注意的是<br>
1>当程序处于启动状态时,`keyWindow`可能为nil,因为此时程序还没有显示出来,13系统以后苹果推荐使用`Scene`来管理窗口<br>
2>当程序处于后台时,`keyWindow`可能为nil,因为此时程序已经被挂起,不再显示<br>
3>程序启动过程中,`keyWindow`可能会发生变化,也可能是nil,因为程序启动过程中,可能会创建多个窗口,最后显示的窗口才是`keyWindow`<br>
具体表现是,在root的控制器中分别在`viewDidload`和`viewDidAppear`打印`keyWindow`:<br>
```swift
"viewDidLoad = nil"
"viewDidAppear = Optional(<UIWindow: 0x100b095a0; frame = (0 0; 414 896);gestureRecognizers = <NSArray: 0x281662b50>; layer = <UIWindowLayer: 0x281662ac0>>)"
```
上面的方法我已经分别在包含`Scene`和不包含两种工程中测试,都已经通过,如果有问题,欢迎指出
{{< /admonition >}}


