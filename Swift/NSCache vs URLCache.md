# NSCache vs URLCache

- NSCache의 대안인 URLCache를 사용하여 이미지 및 기타 미디어 파일을 메모리 또는 스토리지에 저장합니다.

## NSCache

- **키-값 쌍을 임시로 저장**하는 데 사용하는 mutable collection 컬렉션
- **in memory cache** 이기 때문에 **속도에 이상적**이고, 데이터를 **메모리에 저장**하므로 RAM 에서 쉽게 액세스 가능
- 하지만 장치를 끄거나 **앱을 닫으면 RAM 내의 콘텐츠가 종료**되므로 **지속성이 부족**
- **리소스가 부족**할 때는 **제거**되기 쉬움
- NSCache는 **셀 콘텐츠를 빠르게 캐시하고 재사용**해야 하는 **UITableView** 및 **UICollectionView**에 이상적 (위아래로 스크롤하는 경우에 유용)

### URLCache

- **in memory cache** 이자, **on disk cache**. 즉, **메모리 및 디스크에 모두 캐시.**
- **on disk cache 의 장점**은 **앱을 재시작할때도 유지**되어야하는 데이터를 **디스크 공간**(스토리지 및 하드드라이브) 에 저장할 수 있다는 것.
- URLCache는 시스템이 디스크 공간이 부족할 때까지 캐시된 데이터를 유지
- **네트워크 데이터 관리**의 경우 데이터를 캐싱할 때 NSCache 대신 **URLCache를 사용**하는 것이 좋음
- 또한 온디스크 캐시는 메모리에 비해 더 많은 데이터를 저장 가능하므로 바이너리 개체(이미지, 오디오 파일 등)와 같은 **대용량 파일**에 이상적

- 온디스크의 단점은 RAM보다는 훨씬 **느리다**는 것

  

## 출처

- [Image Caching with URLCache](https://levelup.gitconnected.com/image-caching-with-urlcache-4eca5afb543a)
- [To `NSCache` or not to `NSCache`, what is the `URLCache`](https://medium.com/@master13sust/to-nscache-or-not-to-nscache-what-is-the-urlcache-35a0c3b02598)

- [NSCache](https://developer.apple.com/documentation/foundation/nscache)





