# UICollectionView

<!--more-->
## 布局从右到左
### 方式1
数据会从右到左排列
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
### 方式2
数据会从右到左排列
```swift
if #available(iOS 9.0, *) {
    collectionView.transform = CGAffineTransform(scaleX: -1.0, y: 1.0)
}
```
```objc
if (@available(iOS 9.0, *)) {
    _collectionView.transform = CGAffineTransformMakeScale(-1.0, 1.0);
}
```
cell中的内容也需要做相应的变换
```swift
cell.transform = CGAffineTransform(scaleX: -1.0, y: 1.0)
```
```objc
cell.transform = CGAffineTransformMakeScale(-1.0, 1.0);
```
## 方式3
自定义UICollectionViewFlowLayout,这种方式不会改变数据的排列方向，只是改变了cell的布局方向   
比如一行有4个cell，那么第一个cell的x坐标为0，第二个cell的x坐标为第一个cell的x坐标加上第一个cell的宽度，以此类推，第四个cell的x坐标为第三个cell的x坐标加上第三个cell的宽度，这样就可以实现从右到左的布局   
示例代码如下：
```swift
class RTLCollectionViewFlowLayout: UICollectionViewFlowLayout {
    
    override func layoutAttributesForElements(in rect: CGRect) -> [UICollectionViewLayoutAttributes]? {
        guard let attributesArray = super.layoutAttributesForElements(in: rect) else {
            return nil
        }
        var newAttributesArray = [UICollectionViewLayoutAttributes]()
        // 获取余数
        let remainder = attributesArray.count % 4
        // 获取collectionView的宽度
        let collectionViewWidth = collectionViewContentSize.width
        var currentX: CGFloat = sectionInset.left
        if attributesArray.count < 4 {
            // 只有一行
            currentX = collectionViewWidth - sectionInset.right - CGFloat(remainder) * (itemSize.width + minimumInteritemSpacing)
        } 
        var currentY: CGFloat = sectionInset.top
        for (index,attributes) in attributesArray.enumerated() {
            let newAttributes = attributes.copy() as! UICollectionViewLayoutAttributes
            if newAttributes.frame.origin.y == currentY {
                // 在同一行
                newAttributes.frame.origin.x = currentX
            } else {
                // 开始新的一行
                currentY = newAttributes.frame.origin.y
                let last = attributesArray.count - index
                if last < 4 {
                    let count = last % 4
                    currentX = collectionViewContentSize.width - sectionInset.right - CGFloat(count) * itemSize.width - CGFloat(count - 1) * minimumInteritemSpacing
                } else {
                    currentX = sectionInset.left
                }
                newAttributes.frame.origin.x = currentX
            }
            currentX += newAttributes.frame.size.width + minimumInteritemSpacing
            newAttributesArray.append(newAttributes)
        }
        return newAttributesArray
    }
}
```
```objc
@implementation RTLCollectionViewFlowLayout
- (NSArray<UICollectionViewLayoutAttributes *> *)layoutAttributesForElementsInRect:(CGRect)rect {
    NSArray *attributesArray = [super layoutAttributesForElementsInRect:rect];
    NSMutableArray *newAttributesArray = [NSMutableArray array];
    // 获取余数
    NSInteger remainder = attributesArray.count % 4;
    // 获取collectionView的宽度
    CGFloat collectionViewWidth = self.collectionViewContentSize.width;
    CGFloat currentX = self.sectionInset.left;
    if (attributesArray.count < 4) {
        // 只有一行
        currentX = collectionViewWidth - self.sectionInset.right - remainder * (self.itemSize.width + self.minimumInteritemSpacing);
    }
    CGFloat currentY = self.sectionInset.top;
    for (NSInteger index = 0; index < attributesArray.count; index++) {
        UICollectionViewLayoutAttributes *attributes = attributesArray[index];
        UICollectionViewLayoutAttributes *newAttributes = [attributes copy];
        if (newAttributes.frame.origin.y == currentY) {
            // 在同一行
            newAttributes.frame.origin.x = currentX;
        } else {
            // 开始新的一行
            currentY = newAttributes.frame.origin.y;
            NSInteger last = attributesArray.count - index;
            if (last < 4) {
                NSInteger count = last % 4;
                currentX = collectionViewWidth - self.sectionInset.right - count * self.itemSize.width - (count - 1) * self.minimumInteritemSpacing;
            } else {
                currentX = self.sectionInset.left;
            }
            newAttributes.frame.origin.x = currentX;
        }
        currentX += newAttributes.frame.size.width + self.minimumInteritemSpacing;
        [newAttributesArray addObject:newAttributes];
    }
    return newAttributesArray;
}
@end
```



