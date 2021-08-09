# Swift에서 @available로 API 제약을 수행하는 방법

- 더 이상 사용되지 않는 속성

  ```swift
  @available(iOS, deprecated: 11 )
  @available(iOS, deprecated: 11, message: "Use at your own risk" )
  ```
  

- 속성 도입

  ```swift
  @available(iOS, introduced: 12)
  ```

- 이름이 변경된 속성

  ```swift
  @available(iOS, deprecated: 11, renamed: "addByForSureTwo")
  
  ```

- 사용할 수 없는 속성

  ```swift
  @available(iOS, unavailable, message: "This API will be supported and cou
  ```

  

# 출처

- [How to do APIs constraints with @Available in Swift](https://holyswift.app/how-to-do-apis-constraints-with-available-in-swift)

