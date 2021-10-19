# KVC (Key Value Coding)

- 객체의 값을 직접 가져오지않고, Key 또는 KeyPath 를 이용해서 **간접적으로** 데이터를 가져오거나 수정하는 방법

  ```swift
  struct Person {
      var name: String
  }
  
  var naljin = Person(name: "naljin")
  
  // 객체의 값을 "직접"가져오는 방식
  print(naljin.name)
  
  //keyPath를 이용하여 "간접"적으로 값을 가져오는 방식
  naljin[keyPath: \.name]
  naljin[keyPath: \.name] = "sujin" // 값 변경도 가능
  ```

- KeyPath 문법은 `\BaseType.propertyName`

  위의 예시로 들자면 `\Person.name` 이 되지만, 위 경우에는 **`naljin` 이 이미 Person타입이므로 타입을 생략** 해서 표현할 수 있음

- KeyPath 를 정의해서도 사용 가능 

  ```swift
  let writableKeyPath = \Person.name
  
  naljin[keyPath: writableKeyPath] // naljin
  naljin[keyPath: writableKeyPath] = "sujin" //sujin
  ```

# 출처

- [Key-Value Coding(KVC) / KeyPath in Swift](https://zeddios.tistory.com/1218?category=685736)

