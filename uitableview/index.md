# UITableView使用记录

<!--more-->
## UITableView的`type`
### UITableView的类型(type)
```swift
public enum Style : Int, @unchecked Sendable {

    case plain = 0 // 通用类

    case grouped = 1 // 分组模式

    @available(iOS 13.0, *)
    case insetGrouped = 2 // iOS13以后支持的API,给组加圆角,效果如下
}
```

<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202301062145544.png" title="insetGrouped"width="50%">}}
<div style="color:black;"> <b> insetGrouped </b>  </div>
</center>

### iOS15系统之后type为`grouped`时,头部有间距
```swift
if #available(iOS 15.0, *) {
    tableView.sectionHeaderTopPadding = 0
}
```
```objc
if (@available(iOS 15.0, *)) {
    _tableView.sectionHeaderTopPadding = 0;
}
```

### type为`grouped`时,组间距问题
```swift
func tableView(_ tableView: UITableView, heightForFooterInSection section: Int) -> CGFloat {
    return 0
}
    
func tableView(_ tableView: UITableView, viewForFooterInSection section: Int) -> UIView? {
    return UIView()
}
```





