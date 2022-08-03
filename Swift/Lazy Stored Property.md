# Lazy Stored Property

1. `lazy` 속성은 인스턴스가 **초기화된 이후에 개별적으로 초기화**되기 됨.

   즉 초기에는 값이 존재하지 않고 **이후에 값이 생기는 것**이기 때문에 **`let` 으로는 선언 불가.**  ( **상수** 프로퍼티는 언제나 **초기화 완료 이전에 값을 갖**기 때문)

2. 생성자에서 초기화 하지 않기 때문에 선언 시점에 **기본값**을 저장해야 함.


3. **처음 사용될 때 메모리에 값을 올리고** 그 이후 부터는 계속해서 메모리에 **올라온 값을 사용**.

   따라서 사용할 때마다 값을 연산하여 사용하는 **computed property 로 사용 불가**
   
4. multi-thread 환경에서 lazy property에 많은 thread가 비슷한 시점에접근한다면, 여러 번 초기화가 될 수 있음

5. `lazy`에 연산을 통해 값을 초기화할 때 **다른 프로퍼티의 값을 사용**하기 위해서는 `closure`내에서 **`self`를 통해 접근이 가능**

   기본적으로 **일반 프로퍼티**들은 **클래스가 생성된 이후에 접근이 가능**. 그래서 `a` property를 생성할때 `b` property를 `self.b` 처럼 접근할 수 없음.

   하지만  `lazy` 키워드가 붙으면 해당 프로퍼티의 초기화는 **추후에 실행**된다는 의미이기 때문에, `closure`내에서 **`self`로 다른 프로퍼티에 접근**이 가능.

   ```swift
   class Person {
       var name:String
       
       lazy var greeting:String = {
           return "Hello my name is \((self.name))" //greeting 변수가 lazy 로 선언되어있기 때문에 self.name 접근 가능
       }()
     
       init(name:String){
           self.name = name
       }
   }
   ```

5. `struct`, `class` 에서만 사용 가능

# 출처

- [[Swift\] lazy Variables](https://baked-corn.tistory.com/45)
- [[Swift] lazy var ? Lazy Stored Property 에 대하여](https://onelife2live.tistory.com/16)
- [Properties](https://docs.swift.org/swift-book/LanguageGuide/Properties.html)
- ["private lazy var ..." thread safe?](https://developer.apple.com/forums/thread/22767)

