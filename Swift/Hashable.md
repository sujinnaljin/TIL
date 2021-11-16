# Hashable

- **정수 해쉬 값을 제공**하기 위해 `Hasher` 로 **해쉬 되는 타입** (정수 **Hash 값**을 제공하는 타입)
- 값을 해싱한다는 것은 **`Hasher` 타입인 해시 함수에 필수 구성 요소를 제공**하는 것을 의미
- 표준 라이브러리의 많은 **기본 타입이 Hashable을 준수** (String, Int, Double 등)하여 Hash Value를 제공
- 타입의 hashValue 프로퍼티에 의해 제공되는 **hash 값**은, **비교시 동일한 임의의 두 인스턴스**에 대한 **동일한 정수**
- 즉, 같은 타입의 인스턴스인 a와 b의 경우 `a==b` 이면 `a.hashValue == b.hashValue`
- 하지만 반대의 경우는 true가 아닐 수 있는데, **동일한 hash 값**을 가진 두개의 **인스턴스가 서로 동일한 필요는 없음**
- `struct` 의 경우 모든 stored property 가 `Hashable` 을 준수 해야함
- `enum` 의 경우 모든 associated value가  `Hashable` 을 준수해야 함 (associated values 가 없는  `enum` 은 자동으로 Hashable conform)
- **hash 값**은 **프로그램 실행에 따라 동일하지 않을 수도** 있으므로 향후 실행에 사용할 hash값을 저장하지 않는 것이 좋음
- **Set** 이나 **Dictionary Key**가 Hashable이어야 하는 이유 
  - **빠른 검색** - hashValue가 있기 때문에 우리가 찾으려는 원소를 빨리 찾을 수 있음. 해쉬 함수는 동일 인풋에 대해 동일한 아웃풋이 나오기 때문.
  - 생각해보면, Swift의 Set과 Dictionary는 Array와 다르게 "순서"가 없기 때문에, 원소 찾으려면 Hashable 이어야 할 것 같기도 함... 애초에 Set 은 정의가 정렬되지 않은 "별개"의 원소들의 모임 (An unordered collection of unique elements)이기도 하고..??

# 출처 

- [Swift ) Hashable](https://zeddios.tistory.com/498)
- [Hashable](https://developer.apple.com/documentation/swift/hashable)

