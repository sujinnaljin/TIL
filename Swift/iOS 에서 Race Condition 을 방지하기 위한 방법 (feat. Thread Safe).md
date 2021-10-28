# iOS 에서 Race Condition 을 방지하기 위한 방법 (feat. Thread Safe)

- **경쟁 상태**란 **공유 자원**에 대해 **여러 개의 프로세스가 동시에 접근**을 시도할 때 접근의 타이밍이나 순서에 의해 **자료의 일관성을 해치는** 결과 나타날 수 있는 상태
- 경쟁 상태를 **방지**하기 위해서는, 다수의 스레드가 접근해서 문제가 되는 것이므로 **한번에 한개의 스레드만 접근 가능**하도록 처리하면 됨 
- 이렇게 **멀티 스레드 환경**에서 **공유 데이터**가 **한번에 하나의 스레드에 의해서만 접근**된다는 것을 **Thread-safe** 하다라고 표현
- thread-safety 를 보장하기 위해 iOS 에서 아래와 같은 방법들을 사용할 수 있음.

## 1. NSLock

- NSLock은 Foundation이 제공하는 락(lock)의 기본 타입
- 락은 **한번에 한번에 한 스레드만 취득할 수 있는 오브젝트**이며 이것이 크리티컬 섹션 접근을 완벽하게 감시할 수 있게 만들어줌
- 하지만 올바르게 구현하는 것이 어려움

## 2. Dispatch Semaphore

- Semaphore란 **공유 자원에 접근하는 작업의 수를 제한할 때 사용** 
- 공유 자원에 **접근 가능한 (혹은 한번에 실행 가능한) 작업 수를 명시**
- 임계 구역에 **들어갈때**는 semaphore의 **`wait()`** 를 호출하여 semaphore를 **감소**시키고, **나올때**는 **`signal()`** 을 호출하여 semaphore를 **증가**시킴

```swift
// 공유 자원에 접근 가능한 작업 수를 2개로 제한
let semaphore = DispatchSemaphore(value: 2)

for i in 1...3 {
  semaphore.wait() //semaphore 감소
  DispatchQueue.global().async() {
    //임계 구역(critical section)
    print("공유 자원 접근 시작 \(i) 🌹")
    sleep(3)
    print("공유 자원 접근 종료 \(i) 🥀")
    semaphore.signal() //semaphore 증가
  }
}
```

## 3. Serial Queue + sync 조합

- **공유 자원을 읽고 쓰는 작업을 모두 하나의 serial 큐로 보내**서 처리하면 들어온 **task 에 순서**가 생기는, 즉 다수의 스레드에서 **동시에 값을 접근하지 못하**게 하는 상황이 됨

```swift
let serialQueue = DispatchQueue(label: "com.sujinnaljin.serial")
var sharedValue = 0

DispatchQueue.global(qos: .background).async {
    //공유 자원 write
    serialQueue.sync {
        sharedValue = 100
    }
    
    var mySharedValue = 0
    
    //공유 자원 read 후 할당
    serialQueue.sync {
        mySharedValue = sharedValue
    }
    
    print(mySharedValue) //100
}
```

- 여기서 **`sync`** 를 사용하는 이유는 serial queue로 보낸 작업을 기다림으로써 **공유 자원의 제대로 된 값을 얻기 위함**
- 만약 위의 코드를 **`async`** 로 방법을 변경한다면 실행 흐름이 task 를 보내자마자 **바로 return** 이 될 것이고, 맨 마지막줄의 print 값은 `mySharedValue` 의 초기값인 `0` 이 될 수도 있음 
- 물론 main 에서 `sync`를 사용하면 UI가 멈출 수 있기 때문에 **메인 스레드에서는 사용하면 안되**고, 위의 코드처럼 메인 스레드에서 떨어져 여러 스레드에서 동작(ex. `DispatchQueue.global()`)하는 비동기 작업들에 대해 사용해야 함

## 4. Dispatch Barrier + concurrent queue 조합

- dispatch barrier는 concurrent queue가 사용하고 있는 여러 개의 thread 중에서, barrier block의 실행을 위한 **하나의 스레드만 제외하고는 모든 스레드 사용을 막는 것**
- **해당 시점에는 스레드를 하나만 사용**하기 때문에 serial(직렬) 로 **순서 있는 실행**이 가능

```swift
class Falcon {
    let concurrentQueue = DispatchQueue(label: "com.sujinnaljin.concurrent",
                                        attributes: .concurrent)
    
    private var _name: String
    var name: String {
        get {
            //읽기 - sync
            concurrentQueue.sync {
                return self._name
            }
        }
        set {
            //쓰기 - async
            concurrentQueue.async(flags: .barrier, execute: {
                self._name = newValue
            })
        }
    }

    init(name: String) {
        self._name = name
    }
}
```

# 출처

- [Swift Data Race vs. Race Condition](https://karthikmk.medium.com/swift-data-race-vs-race-condition-5d92c389a3ab)
- [[iOS] 차근차근 시작하는 GCD — 10 - DispatchSemaphore 에 대해 알아봅시다](https://sujinnaljin.medium.com/ios-%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-gcd-10-cb37c3e0cf13)
- [[iOS] 차근차근 시작하는 GCD — 11 - 동시성과 관련된 문제들 (Concurrency Problems) 중 경쟁 상태(Race Condition)에 대해 알아봅시다](https://sujinnaljin.medium.com/ios-%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-gcd-11-1d830eeec781)
- [[iOS] 차근차근 시작하는 GCD — 12 - 경쟁 상태(Race Condition) 해결 방법에 대해 알아봅시다 — Serial queue + sync](https://sujinnaljin.medium.com/ios-%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-gcd-12-c06b599fe7f5)
- [[iOS] 차근차근 시작하는 GCD — 13 - 경쟁 상태(Race Condition) 해결 방법에 대해 알아봅시다 — Dispatch Barrier](https://sujinnaljin.medium.com/ios-%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-gcd-13-723d33e157e)
- [[번역]스위프트에서 동시성에대한 모든것-Part1 : 현재편](https://blog.canapio.com/128#NSLock)

