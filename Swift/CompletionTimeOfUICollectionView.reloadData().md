# UICollectionView.reloadData()의 완료 시점

- reloadData() 호출 후 일어나는 일
  - sizeForItemAt
  - insetForSectionAt
  - minimumLineSpacingForSectionAt
  - cellForItemAt
  - layoutSubViews
- 따라서 **콜렉션뷰의 layoutSubviews()는 reloadData()의 완료 시점이라고 생각할 수 있다.** 
- reloadData() 완료 후 실행할 것을 블록으로 만들고 layoutSubviews()에서 그 블록을 실행하면 임의의 시간 지연이라는 불확실성을 제거할 수 있다.

![Image for post](https://miro.medium.com/max/1824/1*kLbix34uyBvypueJ5DU1DQ.png)



![Image for post](https://miro.medium.com/max/1988/1*S886xnNqHsG4Q4n6t1mFfw.png)



# 출처

[UICollectionView.reloadData()가 완료된 시점을 알아내는 안전한 방법](https://medium.com/@jongwonwoo/uicollectionview-reloaddata-%EA%B0%80-%EC%99%84%EB%A3%8C%EB%90%9C-%EC%8B%9C%EC%A0%90%EC%9D%84-%EC%95%8C%EC%95%84%EB%82%B4%EB%8A%94-%EC%95%88%EC%A0%84%ED%95%9C-%EB%B0%A9%EB%B2%95-fa9bac8d0a89)

