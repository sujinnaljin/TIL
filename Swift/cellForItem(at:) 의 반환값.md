   # cellForItem(at:) 의 반환값

- IndexPath 에 있는 셀 반환

- iOS **15 이전** 버전에서는 **셀이 보이지 않거나 indexPath가 범위를 벗어나면 0을 반환**
- iOS **15 이상**에는 **셀이 보이지 않더라도,** 컬렉션 뷰가 IndexPath 에 대한 **prepared cell을 유지**하면 **nil이 아닌 셀**을 반환
- iOS 15 이상에서 컬렉션 뷰는 다음과 같은 상황에서 prepared cell을 유지
  - prepared cell의 캐시에 prefetch 및 retain 되어있지만, 컬렉션 뷰에 아직 표시되지 않았기 때문에 볼 수 없는 셀
  - collection view 가 셀을 표시한 이후에도 prepared cell의 캐시에 계속 보관하는 이유는, 셀이 보이는 영역 근처에 있다가 다시 view로 스크롤 될 수 있기 때문
  - first responder 를 포함하는 cell  (The cell that contains the first responder.)
  - 포커스가 있는 cell (The cell that has focus.)

## 트러블 슈팅,, 근데 이유는 알 수 없는,
-  원래 reloadData 할때 너무 빨리 호출되어서였는지 `cellForItem(at: indexPath)`가 nil 이 반환되는 이슈가 있었음. 
-  그래서 `DispatchQueue.main.asyncAfter(deadline: .now() + 0.5)` 로 감싸면 잘 나오더라?
-  점점 시간을 줄여보다가 급기야 `DispatchQueue.main.asyncAfter(deadline: .now())` 도 잘 동작하기 시작????
-  음?? 스레드 이슈였단 말임?? 하고 `cellForItem(at: indexPath)` 쪽에 브포 찍고 어느 스레드에서 돌아가나 봄
-  흠 메인에서 잘 돌아가고 있는데요????
-  `DispatchQueue.main.async` 으로 바꿔도 잘 됨,, 당연히 `.asyncAfter(deadline: now())` 랑 같을테니까..
-  흠,, 뭐가 문제길래 nil 을 반환하는거람?

# 출처
- [cellForItem(at:)](https://developer.apple.com/documentation/uikit/uicollectionview/1618088-cellforitem)
- [UICollectionView cellForItemAtIndexPath is nil](https://stackoverflow.com/questions/22861804/uicollectionview-cellforitematindexpath-is-nil)
