# UIColor的一些扩展

<!--more-->
## 前言
在阅读`Telegram`源码的过程中,抽出业务代码,留下有用的代码,同时也修改或总结一些自己遇到的好用的方法.
## 颜色

{{< admonition quote "wiki">}}
**红绿蓝三原色 (RGB)**    
发光的媒体（比如电视机）使用红、绿 和蓝加色的三元色，每种光尽可能只刺激针对它们的锥状细胞而不刺激其它的锥状细胞。这个系统的色域占人可以感受到的色彩空间的大部分，因此电视机和电脑萤幕使用这个系统。   
理论上我们也可以使用其他颜色作为元色，但使用红、绿和蓝我们可以最大地达到人的色彩空间。遗憾的是对于红、绿和蓝色没有固定的波长的定义，因此不同的技术仪器可能使用不同的波长从而在萤幕上产生稍微不同的颜色。    
    
**色相、色度和明度系统 (HSB)**    
在制作计算机图像时人们往往使用另一种颜色系统。这个颜色系统使用三项分类，分别叫做色相（hue）、色度（saturation）和明度（brightness）的系数。色调决定到底哪一种颜色被使用，色度决定颜色的纯度，亮度决定颜色的明暗程度

参考[wiki](https://zh.wikipedia.org/wiki/%E9%A2%9C%E8%89%B2)
{{< /admonition >}}

## `UIColor`初始化扩展
`rgb` `argb` `hex`数值或者字符串颜色值的初始化
```swift
convenience init(rgb: UInt64) {
    self.init(red: CGFloat((rgb >> 16) & 0xff) / 255.0, green: CGFloat((rgb >> 8) & 0xff) / 255.0, blue: CGFloat(rgb & 0xff) / 255.0, alpha: 1.0)
}

convenience init(rgb: UInt64, alpha: CGFloat) {
    self.init(red: CGFloat((rgb >> 16) & 0xff) / 255.0, green: CGFloat((rgb >> 8) & 0xff) / 255.0, blue: CGFloat(rgb & 0xff) / 255.0, alpha: alpha)
}

convenience init(argb: UInt64) {
    self.init(red: CGFloat((argb >> 16) & 0xff) / 255.0, green: CGFloat((argb >> 8) & 0xff) / 255.0, blue: CGFloat(argb & 0xff) / 255.0, alpha: CGFloat((argb >> 24) & 0xff) / 255.0)
}

convenience init?(hex: String) {
    let scanner = Scanner(string: hex)
    if hex.hasPrefix("#") {
        scanner.scanLocation = 1
    }
    var value: UInt64 = 0
    if scanner.scanHexInt64(&value) {
        if hex.count > 7 {
            self.init(argb: value)
        } else {
            self.init(rgb: value)
        }
    } else {
        return nil
    }
}
```
{{< admonition tip "位移运算">}}
左移:`<<`,右移:`>>`   
对一个数进行按位左移或按位右移，相当于对这个数进行乘以 2 或除以 2 的运算。将一个整数左移一位，等价于将这个数乘以 2，同样地，将一个整数右移一位，等价于将这个数除以 2。    
十六进制一个字符4位,例如 **#AABBCC** **AA**代表红,**BB**代表绿,**CC**代表蓝,三个值分别都以**255**为基数,要得到**AA**的值,可以按位与`&`**0xFF0000** 取反再右移16位的到**0xAA**即十进制**170**.
{{< /admonition >}}
## 属性扩展
### alpha透明度
```swift
var alpha: CGFloat {
    var alpha: CGFloat = 0.0
    if self.getRed(nil, green: nil, blue: nil, alpha: &alpha) {
        return alpha
    } else if self.getWhite(nil, alpha: &alpha) {
        return alpha
    } else {
        return 0.0
    }
}

```
### RGB值
```swift
var rgb: UInt64 {
    var red: CGFloat = 0.0
    var green: CGFloat = 0.0
    var blue: CGFloat = 0.0
    if self.getRed(&red, green: &green, blue: &blue, alpha: nil) {
        return (UInt64(max(0.0, red) * 255.0) << 16) | (UInt64(max(0.0, green) * 255.0) << 8) | (UInt64(max(0.0, blue) * 255.0))
    } else if self.getWhite(&red, alpha: nil) {
        return (UInt64(max(0.0, red) * 255.0) << 16) | (UInt64(max(0.0, red) * 255.0) << 8) | (UInt64(max(0.0, red) * 255.0))
    } else {
        return 0
    }
}
```
### ARGB值
```swift
var argb: UInt64 {
    var red: CGFloat = 0.0
    var green: CGFloat = 0.0
    var blue: CGFloat = 0.0
    var alpha: CGFloat = 0.0
    if self.getRed(&red, green: &green, blue: &blue, alpha: &alpha) {
        return (UInt64(alpha * 255.0) << 24) | (UInt64(max(0.0, red) * 255.0) << 16) | (UInt64(max(0.0, green) * 255.0) << 8) | (UInt64(max(0.0, blue) * 255.0))
    } else if self.getWhite(&red, alpha: &alpha) {
        return (UInt64(max(0.0, alpha) * 255.0) << 24) | (UInt64(max(0.0, red) * 255.0) << 16) | (UInt64(max(0.0, red) * 255.0) << 8) | (UInt64(max(0.0, red) * 255.0))
    } else {
        return 0
    }
}
```
### HSB值
```swift
var hsb: (h: CGFloat, s: CGFloat, b: CGFloat) {
    var hue: CGFloat = 0.0
    var saturation: CGFloat = 0.0
    var brightness: CGFloat = 0.0
    if self.getHue(&hue, saturation: &saturation, brightness: &brightness, alpha: nil) {
        return (hue, saturation, brightness)
    } else {
        return (0.0, 0.0, 0.0)
    }
}
```
### 明度值
```swift
var lightness: CGFloat {
    var red: CGFloat = 0.0
    var green: CGFloat = 0.0
    var blue: CGFloat = 0.0
    if self.getRed(&red, green: &green, blue: &blue, alpha: nil) {
        return 0.2126 * red + 0.7152 * green + 0.0722 * blue
    } else if self.getWhite(&red, alpha: nil) {
        return red
    } else {
        return 0.0
    }
}
```
### HEX字符串
```swift
var hexString: String {
    return String(rgb, radix: 16, uppercase: false).rightJustified(width: 6, pad: "0")
}
```
**rightJustified**
```swift
extension String {
    func rightJustified(width: Int, pad: String = " ", truncate: Bool = false) -> String {
        guard width > count else {
            return truncate ? String(suffix(width)) : self
        }
        return String(repeating: pad, count: width - count) + self
    }
}
```

## 方法扩展
### 改变亮度明度(根据传入系数使亮度明度乘积得到新颜色)
```swift
func withMultipliedBrightnessBy(_ factor: CGFloat) -> UIColor {
    var hue: CGFloat = 0.0
    var saturation: CGFloat = 0.0
    var brightness: CGFloat = 0.0
    var alpha: CGFloat = 0.0
    self.getHue(&hue, saturation: &saturation, brightness: &brightness, alpha: &alpha)
    
    return UIColor(hue: hue, saturation: saturation, brightness: max(0.0, min(1.0, brightness * factor)), alpha: alpha)
}
```
如下调整`.red`红色亮度比例从左到右分别为**0.3**,**0.6**,**1**(不改变)
```swift
override func viewDidLoad() {
    super.viewDidLoad()
    let bgView1 = UIView(frame: CGRectMake(17, 100, 100, 100))
    bgView1.backgroundColor = .red.withMultipliedBrightnessBy(0.3)
    self.view.addSubview(bgView1)
    
    let bgView2 = UIView(frame: CGRectMake(127, 100, 100, 100))
    bgView2.backgroundColor = .red.withMultipliedBrightnessBy(0.6)
    self.view.addSubview(bgView2)
    
    let bgView3 = UIView(frame: CGRectMake(237, 100, 100, 100))
    bgView3.backgroundColor = .red.withMultipliedBrightnessBy(1)
    self.view.addSubview(bgView3)
}
```
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202302072135805.png" title="withMultipliedBrightnessBy" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> withMultipliedBrightnessBy </b>  </div>
</center>

### 改变HSB值
```swift
func withMultiplied(hue: CGFloat, saturation: CGFloat, brightness: CGFloat) -> UIColor {
    var hueValue: CGFloat = 0.0
    var saturationValue: CGFloat = 0.0
    var brightnessValue: CGFloat = 0.0
    var alphaValue: CGFloat = 0.0
    self.getHue(&hueValue, saturation: &saturationValue, brightness: &brightnessValue, alpha: &alphaValue)
    
    return UIColor(hue: max(0.0, min(1.0, hueValue * hue)), saturation: max(0.0, min(1.0, saturationValue * saturation)), brightness: max(0.0, min(1.0, brightnessValue * brightness)), alpha: alphaValue)
}
```
如下调整`.red`颜色**HSB**值
```swift
private func setupViews2() {
    let bgView1 = UIView(frame: CGRectMake(17, 100, 100, 100))
    bgView1.backgroundColor = .red.withMultiplied(hue: 1.034, saturation: 0.819, brightness: 0.633)
    self.view.addSubview(bgView1)
    
    let bgView2 = UIView(frame: CGRectMake(127, 100, 100, 100))
    bgView2.backgroundColor = .red.withMultiplied(hue: 1.029, saturation: 0.77, brightness: 0.332)
    self.view.addSubview(bgView2)
    
    let bgView3 = UIView(frame: CGRectMake(237, 100, 100, 100))
    bgView3.backgroundColor = .red.withMultiplied(hue: 1.034, saturation: 0.583, brightness: 1.234)
    self.view.addSubview(bgView3)
}
```
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202302151628332.png" title="withMultiplied" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> withMultiplied </b>  </div>
</center>

### 混合颜色(alpha合成)
{{< admonition tip "说明">}}
RGB混合算法目前在常用到的算法是`AlphaBlend`.      
计算公式如下:假设一幅图象是A，另一幅透明的图象是B，那么透过B去看A，看上去的图象C就是B和A的混合图象，设B图象的透明度为alpha(取值为0-1，1为完全透明，0为完全不透明).
`Alpha`混合公式如下：
```
R(C)=(1-alpha)*R(B) + alpha*R(A)
G(C)=(1-alpha)*G(B) + alpha*G(A)
B(C)=(1-alpha)*B(B) + alpha*B(A)
```
R(x)、G(x)、B(x)分别指颜色x的RGB分量原色值。从上面的公式可以知道，Alpha其实是一个决定混合透明度的数值。
这里只对B图的Alpha进行了处理，但A图本身如果也有透明通道的，也需要进行一样的处理，即
```
A(C)=(1-alpha)*A(B) + alpha*A(A)
```
[【验证链接】](https://antv.vision/smart-color/#colorComputation)       
[【wiki】](https://zh.wikipedia.org/wiki/Alpha%E5%90%88%E6%88%90)       
[【参考链接】](https://www.cnblogs.com/oloroso/p/10724803.html#3314521018)
{{< /admonition >}}
**mixedWith**        
```swift
func mixedWith(_ other: UIColor, alpha: CGFloat) -> UIColor {
    let alpha = min(1.0, max(0.0, alpha))
    let oneMinusAlpha = 1.0 - alpha
    
    var r1: CGFloat = 0.0
    var r2: CGFloat = 0.0
    var g1: CGFloat = 0.0
    var g2: CGFloat = 0.0
    var b1: CGFloat = 0.0
    var b2: CGFloat = 0.0
    var a1: CGFloat = 0.0
    var a2: CGFloat = 0.0
    if self.getRed(&r1, green: &g1, blue: &b1, alpha: &a1) &&
        other.getRed(&r2, green: &g2, blue: &b2, alpha: &a2)
    {
        let r = r1 * oneMinusAlpha + r2 * alpha
        let g = g1 * oneMinusAlpha + g2 * alpha
        let b = b1 * oneMinusAlpha + b2 * alpha
        let a = a1 * oneMinusAlpha + a2 * alpha
        return UIColor(red: r, green: g, blue: b, alpha: a)
    }
    return self
}
```
示例如下将0.5透明度的黄色覆盖到红色上面,透过上面的黄色看下面的红色,得到的颜色:
```swift
private func setupViews3() {
    let bgView1 = UIView(frame: CGRectMake(17, 100, 100, 100))
    bgView1.backgroundColor = .red
    self.view.addSubview(bgView1)
    
    let bgView2 = UIView(frame: CGRectMake(127, 100, 100, 100))
    bgView2.backgroundColor = .yellow
    self.view.addSubview(bgView2)
    
    let bgView3 = UIView(frame: CGRectMake(237, 100, 100, 100))
    bgView3.backgroundColor = .red.mixedWith(.yellow, alpha: 0.5)
    self.view.addSubview(bgView3)
}
```
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202302161403519.png" title="mixedWith" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> mixedWith </b>  </div>
</center>

**blitOver**                
和上面的`mixedWith`方法相反,表示覆盖到另一个颜色上面作为前景色.
```swift
func blitOver(_ other: UIColor, alpha: CGFloat) -> UIColor {
    let alpha = min(1.0, max(0.0, alpha))
    
    var r1: CGFloat = 0.0
    var r2: CGFloat = 0.0
    var g1: CGFloat = 0.0
    var g2: CGFloat = 0.0
    var b1: CGFloat = 0.0
    var b2: CGFloat = 0.0
    var a1: CGFloat = 0.0
    var a2: CGFloat = 0.0
    if self.getRed(&r1, green: &g1, blue: &b1, alpha: &a1) &&
        other.getRed(&r2, green: &g2, blue: &b2, alpha: &a2)
    {
        let resultingAlpha = max(0.0, min(1.0, alpha * a1))
        let oneMinusResultingAlpha = 1.0 - resultingAlpha
        
        let r = r1 * resultingAlpha + r2 * oneMinusResultingAlpha
        let g = g1 * resultingAlpha + g2 * oneMinusResultingAlpha
        let b = b1 * resultingAlpha + b2 * oneMinusResultingAlpha
        let a: CGFloat = 1.0
        return UIColor(red: r, green: g, blue: b, alpha: a)
    }
    return self
}
```
### 调整透明度倍数
```swift
func withMultipliedAlpha(_ alpha: CGFloat) -> UIColor {
    var r1: CGFloat = 0.0
    var g1: CGFloat = 0.0
    var b1: CGFloat = 0.0
    var a1: CGFloat = 0.0
    if self.getRed(&r1, green: &g1, blue: &b1, alpha: &a1) {
        return UIColor(red: r1, green: g1, blue: b1, alpha: max(0.0, min(1.0, a1 * alpha)))
    }
    return self
}
```
### 篡改一个颜色到另一个颜色

{{< admonition tip "说明">}}
`fraction`取值范围为**0~1**,当为0时,返回当前颜色,当为1时,返回传入颜色.          
从0到1的变化既是当前颜色到传入颜色的变化.
{{< /admonition >}}

```swift
func interpolateTo(_ color: UIColor, fraction: CGFloat) -> UIColor? {
    let f = min(max(0, fraction), 1)
    
    var r1: CGFloat = 0.0
    var r2: CGFloat = 0.0
    var g1: CGFloat = 0.0
    var g2: CGFloat = 0.0
    var b1: CGFloat = 0.0
    var b2: CGFloat = 0.0
    var a1: CGFloat = 0.0
    var a2: CGFloat = 0.0
    if self.getRed(&r1, green: &g1, blue: &b1, alpha: &a1) &&
        color.getRed(&r2, green: &g2, blue: &b2, alpha: &a2) {
        let r: CGFloat = CGFloat(r1 + (r2 - r1) * f)
        let g: CGFloat = CGFloat(g1 + (g2 - g1) * f)
        let b: CGFloat = CGFloat(b1 + (b2 - b1) * f)
        let a: CGFloat = CGFloat(a1 + (a2 - a1) * f)
        
        return UIColor(red: r, green: g, blue: b, alpha: a)
    } else {
        return self
    }
}
```

### 欧氏距离
{{< admonition tip "说明">}}
很多日常使用的“颜色差异”，是直接通过在一个“设备无关”的色彩空间里，进行欧氏距离的计算得到的。给定一个RGB（红绿蓝）的色彩空间，最简单的差异计算方式就是在这个三维空间里求两个点间的距离.      
有不少人尝试将RGB三值加上权重，希望可以让得到的结果更加符合人类感官。一种做法是使用2、4、3：
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202302201044587.png" title="" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b>  </b>  </div>
</center>       

[【wiki】](https://zh.wikipedia.org/zh-hans/%E9%A2%9C%E8%89%B2%E5%B7%AE%E5%BC%82)
{{< /admonition >}}
```swift
private var colorComponents: (r: Int64, g: Int64, b: Int64) {
    var r: CGFloat = 0.0
    var g: CGFloat = 0.0
    var b: CGFloat = 0.0
    if self.getRed(&r, green: &g, blue: &b, alpha: nil) {
        return (Int64(max(0.0, r) * 255.0), Int64(max(0.0, g) * 255.0), Int64(max(0.0, b) * 255.0))
    } else if self.getWhite(&r, alpha: nil) {
        return (Int64(max(0.0, r) * 255.0), Int64(max(0.0, r) * 255.0), Int64(max(0.0, r) * 255.0))
    }
    return (0, 0, 0)
}

func distance(to other: UIColor) -> Int64 {
    let e1 = self.colorComponents
    let e2 = other.colorComponents
    let rMean = (e1.r + e2.r) / 2
    let r = e1.r - e2.r
    let g = e1.g - e2.g
    let b = e1.b - e2.b
    let a1 = ((512 + rMean) * r * r) >> 8
    let b1 = 4 * g * g
    let c1 = ((767 - rMean) * b * b) >> 8
    return a1 + b1 + c1
}
```

### 平均颜色
```swift
static func average(of colors: [UIColor]) -> UIColor {
    var sr: CGFloat = 0.0
    var sg: CGFloat = 0.0
    var sb: CGFloat = 0.0
    var sa: CGFloat = 0.0

    for color in colors {
        var r: CGFloat = 0.0
        var g: CGFloat = 0.0
        var b: CGFloat = 0.0
        var a: CGFloat = 0.0
        color.getRed(&r, green: &g, blue: &b, alpha: &a)
        sr += r
        sg += g
        sb += b
        sa += a
    }

    return UIColor(red: sr / CGFloat(colors.count), green: sg / CGFloat(colors.count), blue: sb / CGFloat(colors.count), alpha: sa / CGFloat(colors.count))
}
```
