# selectItem 은 didSelectItemAt 을 호출하지 않음

`selectItem(at:animated:scrollPosition:)` 은 어떤 selection-related delegate 도 야기하지 않음

따라서 didSelectItemAt 은 selectItem 에서 호출되지 않고, 아래처럼 직접 호출해야함

```swift
self.collectionView(self.collectionView, didSelectItemAt: IndexPath(item: 0, section: 0))
```

# 출처

- [Can't select item in UICollectionView programmatically](https://stackoverflow.com/questions/45412881/cant-select-item-in-uicollectionview-programmatically)
- [selectItem(at:animated:scrollPosition:)](https://developer.apple.com/documentation/uikit/uicollectionview/1618057-selectitem)

