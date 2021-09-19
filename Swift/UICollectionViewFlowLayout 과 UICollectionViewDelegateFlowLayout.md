# UICollectionViewFlowLayout 과 UICollectionViewDelegateFlowLayout

## UICollectionViewFlowLayout
- `UICollectionViewFlowLayout` 는 **collection view layout의 한 종류**
- 각 섹션에 대한 header 및 footer view (옵션)를 사용하여 **item을 그리드로 구성**하는 레이아웃 object
- 일종의 **default flow layout** 으로, 대부분의 경우에 적용 됨
- 만약 `UICollectionViewFlowLayout` 에서 처리할 수 없는 것이 필요한 경우 `UICollectionViewLayout` 을 subclass하여 추가 기능 제공 가능


## UICollectionViewDelegateFlowLayout
- grid-based 레이아웃을 구현하기 위해 기존의 **`UICollectionViewFlowLayout` 객체와 상호 작용**할 수 있는 메소드(optional)를 정의한 프로토콜. 
- 이러한 메소드를 통해 item의 **사이즈**나 **간격**을 조정할 수 있음.



# 출처

- [Difference between UICollectionViewDelegateFlowLayout UICollectionViewFlowLayout](https://stackoverflow.com/questions/24047284/difference-between-uicollectionviewdelegateflowlayout-uicollectionviewflowlayout)

- [[iOS\] UICollectionView 톺아보기 - 3](https://k-elon.tistory.com/26)
- [UICollectionViewFlowLayout](https://developer.apple.com/documentation/uikit/uicollectionviewflowlayout)

