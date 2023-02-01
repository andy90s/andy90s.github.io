# iOS中的只读属性

<!--more-->
## OC只读,Swift设置set私有`private(set)`
```swift
public private(set) var someKey: String
```
当外部调用set方法时的报错信息:
```
Cannot assign to property: 'somekey' setter is inaccessible
```
OC表示只读`readonly`:
```Objective-C
@property (nonatomic, strong, readonly) NSString *someKey;
```

