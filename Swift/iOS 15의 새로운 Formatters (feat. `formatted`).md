# iOS 15의 새로운 Formatters (feat. `formatted`)

- 이전의 formatter 작성 방식은 몇가지 단점이 있음 
  - 인스턴스화하고 유지 관리하는 데 비용이 많이 들 수 있으며, 반복 또는 기타 상황에서 올바르게 사용되지 않으면 **속도가 느려지고 리소스가 과도**하게 사용 됨
  - 종종 **특정 구문**(날짜 형식 등)이 필요하고, 일반적으로 **여러 줄의 코드**를 통해 `Formatter` 개체에 여러 속성을 설정 -> 코딩 프로세스를 더 **복잡**하고 / **느리게** 만들고 **유지 관리하기 어려**움

- iOS 15에서 등장한 **새로운 API**는 **포맷터 생성 프로세스를 제거**하여 간단하게 함

  - 캐시해야 하는지 여부에 대해 걱정할 필요가 없음
  - 값을 따로 전달하거나 설정할 필요가 없음

- Apple은 **formatting 을 지원하는 모든 데이터 유형**에 대해 **`.formatted`** 라는 새로운 인스턴스 메소드를 도입

  ```swift
  //전
  let dateFormatter = DateFormatter()
  dateFormatter.dateStyle = .long
  dateFormatter.timeStyle = .none
  let formattedDate = dateFormatter.string(from: Date())
  // August 14, 2021
  
  //후
  Date().formatted(.dateTime.month(.wide).year().day())
  // August 14, 2021
  ```

- 사용 예시

  - **날짜**

    ```swift
    Date().formatted()
    // 6/28/2021, 1:38 PM
    
    Date().formatted(date: .long, time: .omitted)
    // June 28, 2021
    
    Date().formatted(.dateTime.year())
    // Jun 2021
    ```

  - **숫자**

    ```swift
    0.2.formatted()
    // 0.2
    
    0.2.formatted(.number.precision(.significantDigits(2)))
    // 0.20
    
    1.5.formatted(.currency(code: "thb"))
    // THB 1.50
    ```

  - **목록**

    ```swift
    ["Alice", "Bob", "Trudy"].formatted()
    // Alice, Bob, and Trudy
    
    ["Alice", "Bob", "Trudy"].formatted(.list(type: .or))
    // Alice, Bob, or Trudy
    ```

- 새로운 `ParseableFormatStyle` 프로토콜을 확장하여 Custom Formatter도 생성 가능



# 출처

- [Creating Custom Parseable Format Styles in iOS 15](https://emptytheory.com/2021/08/14/creating-custom-parseable-format-styles-in-ios-15/)
- [New Formatters in iOS 15: Why do we need another formatter](https://sarunw.com/posts/new-formatters-in-ios15/)

