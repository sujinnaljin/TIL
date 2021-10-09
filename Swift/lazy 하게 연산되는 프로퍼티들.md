# lazy 하게 연산되는 프로퍼티들

1. **Lazy Stored Properties**

   lazy stored property는 **처음 사용할 때까지 초기 값이 계산되지 않**는 속성.

   lazy stored property 앞에 `lazy` 키워드 를 사용해 정의 가능

2. **Global Variables**

   global 상수 및 변수는 [Lazy Stored Properties](https://docs.swift.org/swift-book/LanguageGuide/Properties.html#ID257)와 유사한 방식으로 항상 **lazy 하게 계산**되지만,  **`lazy` 키워드로 표시할 필요가 없**음.

   local 상수와 변수는 절대 lazy 하게 계산되지 않음.

3. **Type Properties**

   Stored type properties 는 **처음 사용할 때 lazy 하게 초기화** 되지만 **`lazy` 라는 키워드**로 표시할 필요 없음.

   여러 개의 스레드로 동시에 접근해도 한 번만 초기화 됨

   

# 출처

- [Properties - Global and Local Variables](https://docs.swift.org/swift-book/LanguageGuide/Properties.html#ID263)

- [Swift globals and static members are atomic and lazily computed](https://www.jessesquires.com/blog/2020/07/16/swift-globals-and-static-members-are-atomic-and-lazily-computed/)
