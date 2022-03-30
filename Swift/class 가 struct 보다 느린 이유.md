# class 가 struct 보다 느린 이유

## 1. heap 메모리 사용

- heap은 stack보다 dynamic 한 메모리를 할당 방법을 사용
  - heap 영역에서 사용하지 않은 블록을 찾아서 메모리 할당을 진행 (**검색**)
  - 또한 사용이 끝났다면 해제를 해줘야 하는데, 해제를 하려면 해당 메모리를 적절한 위치에 다시 re-insert (**재삽입**)
- 하지만 위에서 살펴본 검색 및 재삽입은 heap 할당과 관련된 주요 cost가 아님.
- heap에서 드는 **가장 큰 비용**은 **locking 또는 동기화 메커니즘**의 필요성에 있음
  - 여러 쓰레드에서 **동시에 heap에 메모리를 할당**할 수 있기 때문에 locking 또는 동기화 메커니즘을 이용해 **Thread-Safe하게 무결성**을 보호해야 함. 그래서 검색 및 재 삽입 전에 lock 이 발생
    - class 인스턴스를 생성할 때 Swift는 heap을 lock 하고, 사용하기 적합한 사이즈의 unused block을 검색
    - 사용이 완료되면 Swift는 우리를 대신해 heap을 lock 하고, 메모리를 해제해 다시 사용하지 않는 블록으로 돌려놓은 뒤에 적절한 위치로 재삽입

## 2. Reference Counting 사용

- Reference Count 는 단순히 정수를 증가하고 감소하는 것보다 간접적이고 복잡한 몇 가지 단계를 거침
- 여러 스레드에서 동시에 heap에 있는 동일한 instance에 접근할 수 있기 때문에 **원자적으로 Reference Count의 감소와 증가**를 진행해야 함
- 이는 **lock** 이 일어난다는 것을 의미 -> Reference Counting 연산이 많이 일어나면 이에 대한 비용 증가

## 3. Dynamic Dispatch 사용

- Dynamic Dispatch는 컴파일러가 **컴파일 타임에 어떤 구현을 실행할 건지 결정 할 수 없음**

  > cf)
  >
  > 반대로 컴파일 타임에 어떤 구현을 실행하도록 결정할 수 있다면 이를 Static Dispatch라고 함. 
  >
  > 컴파일러는 실제로 어떤 구현이 호출될 것인지에 대해 미리 알고 있기 때문에 inlining기법과 같은 것을 활용해 코드를 적극적으로 최적화 가능
  >
  > **인 라이닝**(inlining)은 메소드를 호출할 때 실제 메소드를 호출하지 않고 바로 결과값을 돌려주는 수동 또는 컴파일러 최적화 기법
  >
  > 컴파일러는 Static Dispatch Chain을 inlining 기법을 통해 콜 스택 오버헤드가 발생하지 않는 단일 구현으로 축소 가능
  >
  > ```swift 
  > // inlining 이전 
  > func process(value: Int) {
  >  return 2 * value;
  > }
  > 
  > func foo(a: Int) {
  >  return process(a);
  > }
  > 
  > // inlining 이후
  > func foo(a: Int) {
  >  return 2 * a; // the body of process() is copied into foo()
  > }
  > ```

- Dynamic Dispatch 자체는 Static Dispatch보다 많은 비용을 필요로 하지는 않음. Thread 동기화 오버헤드, Reference Counting, heap 할당과 같은 오버헤드는 없음

- 따라서 성능의 큰 차이를 만들진 않지만 Method Chaining과 같은 다른 것들을 할 때 컴파일러가 수행하는 **inlining기법과 같은 최적화를 막을 수** 있음

  Static Dispatch 에서 컴파일러는 실제로 어떤 구현이 호출될 것인지에 대해 미리 알고 있기 때문에 inlining 기법 활용 가능.

  하지만 Dynamic Dispatch 는 알 수 없기 때문에 런타임에 실제로 구현이 되어있는 곳을 확인하고 jump 함

  실제 **어떤 method 를 호출하는지 결정**하기 위해서, 컴파일러는 class에 있는 **type 필드**를 이용 (class 는 메모리를 관리를 위해 동일한 구조의 struct 보다 **type** 과 **refCount** 를 위한 두 개의 필드가 더 추가 됨) 

  컴파일러는 type 에 해당하는 V-Table(virtual method table)을 보고, 맞는 구현을 실행

  *`d.draw()` -> `d.type.vtable.draw(d)`*
  <img width="914" alt="image" src="https://user-images.githubusercontent.com/20410193/160824036-ce21862f-dbfd-46b1-82d1-1f2011ea2549.png">
  
  # 출처

- [Swift 성능 이해하기 struct-class](https://noah0316.github.io/Swift/2021-12-21-swift-%EC%84%B1%EB%8A%A5-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-struct-class/)


