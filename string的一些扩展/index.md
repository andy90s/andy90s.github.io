# String的一些扩展

<!--more-->
## 前言
在阅读`Telegram`源码的过程中,抽出业务代码,留下有用的代码,同时也修改或总结一些自己遇到的好用的方法.

## 货币字符串

### 数字格式化
去除其他字符,只留数字
```swift
public func numeralFormat() -> String {
    return replacingOccurrences(of:"[^0-9]", with: "", options: .regularExpression)
}
```
### 判断是否是表示0的字符串
比如 **"0"**,**"0.0"** 字符串表示的值都是**0**
```swift
public var representsZero: Bool {
    return numeralFormat().replacingOccurrences(of: "0", with: "").count == 0
}
```
### 是否包含数字
```swift
public var hasNumbers: Bool {
    return numeralFormat().count > 0
}
```
### 字符串最后的索引到最后一个数字的偏移
从结束索引到字符串最后一个数字之后的索引的偏移量。     
例如，对于字符串"123some"，最后一个数字位置是4，因为从字符串结束索引到**3**的索引        
偏移量为4，"e, m, o and s"
```swift
public var lastNumberOffsetFromEnd: Int? {
    guard let indexOfLastNumber = lastIndex(where: { $0.isNumber }) else { return nil }
    let indexAfterLastNumber = index(after: indexOfLastNumber)
    return distance(from: endIndex, to: indexAfterLastNumber)
}
```

### 更新货币字符串的十进制分隔符位置
**decimalDigits**:货币格式化字符串的小数位数
```swift
mutating public func updateDecimalSeparator(decimalDigits: Int) {
    guard decimalDigits != 0 && count >= decimalDigits else { return }
    let decimalsRange = index(endIndex, offsetBy: -decimalDigits)..<endIndex
    
    let decimalChars = self[decimalsRange]
    replaceSubrange(decimalsRange, with: "." + decimalChars)
}
```
## 补齐字符串
### 十六进制对齐
**右对齐**
```swift
func rightJustified(width: Int, pad: String = " ", truncate: Bool = false) -> String {
    guard width > count else {
        return truncate ? String(suffix(width)) : self
    }
    return String(repeating: pad, count: width - count) + self
}
```
**左对齐**
```swift
func leftJustified(width: Int, pad: String = " ", truncate: Bool = false) -> String {
    guard width > count else {
        return truncate ? String(prefix(width)) : self
    }
    return self + String(repeating: pad, count: width - count)
}
```
例如联动 [Color的十六进制字符串]({{<relref "../UIKit/UIColor的一些扩展.md#文章目录##前言">}})
以黑色(**#000000**)为例: 
```swift
String(UIColor.black.rgb, radix: 16) // print 0
```
```swift
String(UIColor.black.rgb, radix: 16, uppercase: false).rightJustified(width: 6,pad: "0") // print 000000
```



