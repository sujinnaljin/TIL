# selectItem(at:animated:scrollPosition:)
- 해당 메소드는 선택과 관련 있는 어떤 delegate 메소드도 호출하지 않는다.
- 따라서 필요한 delegate method를 직접 호출해야한다
```
self.collectionView.selectItem(at: IndexPath(item: 0, section: 0), animated: true, scrollPosition: .bottom)
self.collectionView(self.collectionView, didSelectItemAt: IndexPath(item: 0, section: 0))
```

# 출처
- [Can't select item in UICollectionView programmatically](https://stackoverflow.com/questions/45412881/cant-select-item-in-uicollectionview-programmatically)
- [selectItem(at:animated:scrollPosition:)](https://developer.apple.com/documentation/uikit/uicollectionview/1618057-selectitem)
