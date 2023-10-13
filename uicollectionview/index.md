# UICollectionView

<!--more-->
## 布局从右到左
```swift
if #available(iOS 9.0, *) {
    collectionView.semanticContentAttribute = .forceRightToLeft
}
```
```objc
if (@available(iOS 9.0, *)) {
    _collectionView.semanticContentAttribute = UISemanticContentAttributeForceRightToLeft;
}
```

