# collectionView 호출 순서 (feat. DataSource & DelegateFlowLayout)

1. **numberOfSections**(in collectionView: UICollectionView) -> Int
2. collectionView(_ collectionView: UICollectionView, **numberOfItemsInSection** section: Int) -> Int
3. collectionView(_ collectionView: UICollectionView, **layout** collectionViewLayout: UICollectionViewLayout, **sizeForItemAt** indexPath: IndexPath)
4. collectionView(_ collectionView: UICollectionView, **layout** collectionViewLayout: UICollectionViewLayout, **insetForSectionAt** section: Int) -> UIEdgeInsets
5. collectionView(_ collectionView: UICollectionView, **layout** collectionViewLayout: UICollectionViewLayout, **minimumInteritemSpacingForSectionAt** section: Int) -> CGFloat
6. collectionView(_ collectionView: UICollectionView, **layout** collectionViewLayout: UICollectionViewLayout, **minimumLineSpacingForSectionAt** section: Int) -> CGFloat 
7. collectionView(_ collectionView: UICollectionView, **cellForItemAt** indexPath: IndexPath) -> UICollectionViewCell



# 출처

- [ios CollectionView 예제 - 프로젝트 생성](https://qteveryday.tistory.com/117)

- [UICollectionView.reloadData()가 완료된 시점을 알아내는 안전한 방법](https://jongwonwoo.medium.com/uicollectionview-reloaddata-%EA%B0%80-%EC%99%84%EB%A3%8C%EB%90%9C-%EC%8B%9C%EC%A0%90%EC%9D%84-%EC%95%8C%EC%95%84%EB%82%B4%EB%8A%94-%EC%95%88%EC%A0%84%ED%95%9C-%EB%B0%A9%EB%B2%95-fa9bac8d0a89)
