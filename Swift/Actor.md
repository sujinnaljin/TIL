# Actor

## TL; DR;

- Actor은 concurrency system발생하는 data race 문제를 해결하는 데 사용할 수 있는 또 다른 방법 (mutex, lock 과 같은)
- actor은 actor isolation개념을 사용하여 data race를 방지
- actor가 data race에 도움이 될지라도, race condition이 발생할 수 있는 지점이 있음. 따라서 suspension point 에 도달할 때마다, actor의 state에 대한 어떠한 가정도 하지 않기
- `@MainActor` 을 사용 하면 `DispatchQueue.main` 호출 없이 main 스레드에서 property에 접근할 수 있지만`async/await` 를 같이 사용해야함

## Actor란

- **concurrency**의 가장 큰 문제 중 하나는 **race condition**에 의한 **공유 상태**의 문제. 

- 이를 해결하기 위해 **Lock, 뮤텍스, 공유 데이터에 대한 직렬화된 액세스** 등 다양한 **동시성 모델**이 존재함

- **Actor model**도 이러한 **concurrency model** 중 하나 

- **actor**은 **변경(change/mutation)을 수행할 수 있는 유일한 모델**로서, local **state를 유지, 보호**하는 역할을 함

- 외부 구성원(outside memeber)은 actor에게 자신의 상태에 따라 행동하도록 요청할 수 있으며, **actor은 자신의 상태에 대한 모든 접근/변경 요청을 동기화**하도록 보장

- Actor 모델의 통신에 대한 가장 좋은 설명 중 하나는 다음과 같음.

  > 각 액터가 섬과 같고 우리의 코드 베이스가 섬이 있는 세계라고 상상해 보세요. 이제 각 섬은 병에 담긴 메시지를 보내 다른 섬과 대화할 수 있습니다. 각 섬은 메시지를 보낼 위치(즉, 다른 섬의 주소)를 알고 있으며 이것이 각 섬 간의 통신이 작동하는 방식입니다.

- actor은 **primitive structure**로, class나 struct를 정의하는 것과 동일하게 정의 가능

  - 아래 BankAccount가 class로 정의되었다면 balance 변수는 동시성 환경에서 race condition이 발생할 수 있는 mutable state 겠지만, actor로 선언되었기 때문에 data race로 부터 안전

  ```swift
  actor BankAccount {
    let id = UUID()
    private var balance: Double = 0.0
    
    func send(_ amount: Double, to destination: BankAccount) async throws {
        guard amount >= 0 else {
          throw Error.negativeAmountTransfer
        }
      
        if (amount > balance) {
          throw Error.insufficientFunds
        }
        else {
          self.balance -= amount
          // 만약 destination.balance += amount 로 쓰면 컴파일 에러남
          // the `balance` variable can only be accessed via `self` reference
          await destination.deposit(amount)
        }
    }
    
    func deposit(_ amount: Double) {
        guard amount >= 0 else {
          throw Error.negativeAmountTransfer
        }
        self.balance += amount
    }
  }
  ```

## Actor isolation

- data race 에 대한 보호는 **Actor isolation** 라는 개념을 통해 발생. 이는 **cross-actor memeber(func, property) 에 대한 접근이 어떻게 동작**해야 하는지 정의하는 데 사용되는 용어로, 규칙은 다음과 같음

  - 액터는 **자신의 properties를 읽거나**(read) **동기적**으로 **자신의 fuction을 호출** 가능
  - 액터는 오직 **자신의 properties 만 업데이트** 가능 (아마 synchronously 하게 할거임). 이는 **`self`** 키워드를 사용해서만 **속성 업데이트를 수행**할 수 있음을 의미. 다른 액터의 속성을 업데이트하려고 하면 컴파일러 오류 발생
  - **Cross-actor** property 읽기 또는 함수 호출은 **`await` 키워드를 사용하여 비동기적**으로 발생해야 함. 그러나 immutable properties(`let` 으로 선언된 것) 에 대한 cross-actor 읽기는 동기식(synchronously)으로 발생할 수 있음

  ```swift
  // 1. 자신의 property에 대한 read/write 작업은 동기, 비동기로 할 수 있음
  extension BankAccount {
      func withdraw(amount: Double) async throws -> Double {
          guard amount <= self.balance else { // Sync read on `self.balance` ✅
              throw BankError.insufficientFunds
          }
          self.balance -= amount // Sync Update on `self.balance` ✅
          return amount
      }
  }
  
  // 2. 다른 actor의 property에 대한 read 작업은 await 키워드와 함께 aysnc 하게 가능. 
  func checkIfRich(_ acc: BankAccount) async -> Bool {
      return await acc.balance >= 1000 // ✅
  }
  
  // 3. self 키워드로만 write 작업 가능
  func send(amount: Double, to acc: BankAccount) {
      await acc.balance += amount // ❌ , 컴파일 에러 - actor properties can only be updated from within the actor
  }
  
  // 4. 모든 cross actor function references 는 async로 작업되어야하기 때문에 await 키워드 사용 필요
  func send(money: Double, to acc: BankAccount) async {
      await acc.deposit(money) // ✅
  }
  ```

## Actor re-entrancy

- actor에서 함수 실행 (fuction executions)은 **재진입**(re-entrant)을 의미

- 여기서 재진입이란 runtime이 **중단 지점(suspension point)에서 재진입**해서 거기서부터 **다시 작업을 수행**할 수 있다는 것

- 즉 actor에서 **재진입**(re-entrancy)이란 **function이 중간에 일시 중단** 되고, 해당 함수를 수행하던 **동일한 스레드**가 **다른 task 를 수행**한 다음 **일시 중단된 지점에서 기능을 재개**한다는 의미

  ```swift
  actor BankAccount {
    // ....
    var isOpen = true
    
    //은행 계좌를 폐쇄하려는 코드
     func close() async throws -> Double {
       //이미 폐쇄된 계정을 폐쇄하는 것은 의미가 없으므로 isOpen 여부 확인
          if isOpen {
              do {
                //은행 서버에 계정 폐쇄 요청 (시간 걸릴 수 있음)
                  try await BankServer.requestToClose(self.id)
                //계정이 아직 열려있는지 확인
                  if self.isOpen {
                    //열려있으면 닫고 잔액 반환
                      self.isOpen = false
                      return self.balance
                  } else {
                    //네트워크 호출(은행 서버) 진행 동안 이미 계정 폐쇄를 위한 다른 요청이 들어옴
                      throw BankError.alreadyClosed
                  }
              } catch {
                  throw BankError.cannotClose
              }
          } else {
              throw BankError.alreadyClosed
          }
      }
  }
  ```

- 위의 close 함수는 은행 계좌를 폐쇄하려는 코드로, 이를 위해 은행 서버와 통신 필요.

  여기서 **은행 서버와의 통신**을 위해 잠시 **일시 중단(suspended)** 됨. 그리고 **동일한 스레드**에서 **다른 작업을 수행**한 다음, 은행 서버로부터 **응답을 받으면 이전 작업을 재개**함

- 따라서 모든 **await** 호출은 잠재적인 **일시 중단 지점**
- 은행 서버로부터의 응답을 기다리는 동안 해당 스레드는 `withdraw` , `deposit` , 다른  `cancellation` 요청 등을 처리할 수 있음 
- 따라서 은행 서버가 **응답을 완료**하면 **actor의 상태**가 **정지 지점(suspension point)와 같지 않을 수** 있음. 이는 즉, **await 호출 전후에 액터의 상태에 대해 가정할 수 없다**는 걸 의미 -> 따라서 await 호출 이후에 isOpen 한번 더 체크함

- 그러므로 actor re-entrancy 에 대해 두가지를 기억해야함
  - 항상 sync 코드에서 state 변경하기 (내부 상태를 변경하는 function 에서는 aysnc 함수 호출 피하기)
  - state를 변경하는 함수 내에서 aysnc function 을 호출해야하는 경우에는, await가 완료된 후 해당 state에 대해 어떠한 가정도 하지 않기 

## @MainActor

- `@MainActor` 을 사용 하면 해당 속성에 대한 모든 read/write가 **main thread에서 발생**

- properties, functions,  class/struct 정의부에 해당 property wrapper 사용 가능

- `UILabel`, `UIView` 같은 UIKit class들은 이미 해당 property wrapper 가 적용 됨

- 여기서 유일한 함정은 `async/await` 를 같이 사용해야지만 main thread에서 접근된다는 것

  ```swift
  @MainActor class Person {
      var firstName: String
      var lastName: String
      
      init(firstName: String, lastName: String) {
          self.firstName = firstName
          self.lastName = lastName
      }
      
      func tryToPrintNameOnMainThread() {
          print("Is Main Thread: \(Thread.isMainThread)")
          print("\(firstName) \(lastName)")
      }
  }
  
  let me = Person(firstName: "Neel", lastName: "Bakshi")
  DispatchQueue.global().async {
      print("Currently on main thread: \(Thread.isMainThread)") // Currently on main thread: false
      me.tryToPrintNameOnMainThread() // ❌ Is Main Thread: false
  }
  
  asyncDetached {
      print("Currently on main thread: \(Thread.isMainThread)") // Currently on main thread: false
      await me.tryToPrintNameOnMainThread() // ✅ Is Main Thread: true
  }
  ```

  

# 출처

- [A Deep Dive Into Actors in Swift 5.5](https://betterprogramming.pub/a-deep-dive-into-actors-in-swift-5-5-8cc2fa004ded)

