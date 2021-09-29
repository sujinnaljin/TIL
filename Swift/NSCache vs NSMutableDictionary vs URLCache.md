# NSCache vs NSMutableDictionary vs URLCache

## NSCache

- **키-값 쌍을 임시로 저장**하는 데 사용하는 mutable collection 컬렉션

- **in memory cache** 이기 때문에 **속도에 이상적**이고, 데이터를 **메모리에 저장**하므로 RAM 에서 쉽게 액세스 가능

- 하지만 장치를 끄거나 **앱을 닫으면 RAM 내의 콘텐츠가 종료**되므로 **지속성이 부족**

- **리소스가 부족**할 때는 **제거**되기 쉬움

- NSCache는 **셀 콘텐츠를 빠르게 캐시하고 재사용**해야 하는 **UITableView** 및 **UICollectionView**에 이상적 (위아래로 스크롤하는 경우에 유용)

- **Thread-Safe** 하기 때문에 여러 스레드에서 접근할 때에도 Cache에 **lock을 걸어줄 필요가 없음**

- **Linked List와 Dictionary** 자료구조를 함께 사용

  - **Linked List** 활용 - 캐싱 과정에서 중간에 있는 **값을 추가하거나 삭제**하는 과정이 **빈번**하게 발생하기 때문에 **배열을 사용**하면 데이터를 **앞으로 당기거나 뒤로 미는 작업이 매우 용이**해짐
  - **Dictionary** 활용 - **Key**로 데이터를 접근하면 **O(1)**로 빠르게 탐색 가능

  

### NSCache vs NSMutableDictionary

NSCache 및 NSMutableDictionary **기능과 사용법은 기본적으로 동일**하지만 NSMutableDictionary과는 **다른 몇 가지 특성**을 가짐

- NSCache는 **캐시 할당량을 지정**할 수 있고, 캐시가 시스템 메모리를 너무 많이 사용하지 않도록 **자동 삭제** 함. 다른 응용프로그램에서 메모리가 필요한 경우 캐시에서 일부 항목을 제거하여 메모리 사용 공간을 최소화.  NSMutableDictionary는 시스템이 "메모리 부족" 을 띄울 때 수동으로 삭제 필요
- NSCache는 **스레드 세이프**함. 캐시를 **잠글(lock) 필요 없이** 캐시 항목을 추가하고, 삭제하고 검색할 수 있음
- NSCache는  **키 객체를 복사하지 않고 유지(retain as Strong Reference)** 하기 때문에 key 는 **NSCopying** protocol을 **implement 할 필요가 없**음. 키를 복사하지 않는 이유는 다음과 같음
  - 대부분의 경우 **키는 copy operation을 지원하지 않는 object로 사용** 되기 때문
  - 따라서 NSCache는 키를 자동으로 복사하지 않으므로, **dictionary 보다 편리하게 사용**할 수 있음

- 참고로 **NSDictionary**의 경우, **Key** 는 복사 후 사용되어야 하기 때문에 **NSCopying 프로토콜을**  따라야 함

  

## URLCache

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
- [이미지 캐시 처리와 NSCache](https://beenii.tistory.com/187)

- [NSCache](https://caution-dev.github.io/ios/2019/04/07/NSCache.html)
- [iOS NSCache usage](https://blog.titanwolf.in/a?ID=00400-36843d48-48eb-4ea2-8752-5748066a7a1a)
- [NSMutableDictionary](https://developer.apple.com/documentation/foundation/nsmutabledictionary)

- [The latest interview questions of iOS senior engineer in 2021_ Basic knowledge](https://www.fatalerrors.org/a/the-latest-interview-questions-of-ios-senior-engineer-in-2021_-basic-knowledge.html)

- [Choose NSCache instead of NSDictionary when building the cache](https://blog.spacepatroldelta.com/a?ID=00550-ffc4c00e-07e1-4470-b769-41a6d468dd29)

