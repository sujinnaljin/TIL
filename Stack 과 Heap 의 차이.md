# Stack 과 Heap 의 차이

- **Stack** 은 **static memory allocation**에 사용 됨

  **Heap** 은 **dynamic memory allocation** 에 사용 됨. 힙을 통해 **다른 개체에서 참조하는 데이터를 저장**. 일반적으로 시스템이 **메모리 블록을 요청**하고 **동적으로 할당**할 수 있는 **큰 메모리 풀**

  <img width="642" alt="image" src="https://user-images.githubusercontent.com/20410193/156297066-8d4fd0de-4b63-421b-a0d9-d52b512a617e.png">

- **스택**에 할당된 변수는 **메모리에 직접 저장**되고, **컴파일 시간**에 **할당**. 

  **힙**에 할당된 변수는 **런타임**에 **메모리를 할당**. 

- **스택**은 항상 **LIFO** 순서로 **reserve** 됨.  가장 최근에 reserve 된 블록은 항상 해제될 다음 블록. 이렇게 하면 스택을 추적하는 것이 간단해지며 스택에서 **블록을 해제**하는 것은 단지 **포인터 하나를 조정**하는 것.

  **힙**은 **언제든지** 어느 element / block 든지 **할당 및 해제** 가능. 따라서 어떤 block 이 **비어있고, 할당되어 있는지 판단**하는 것은 매우 **복잡**. 힙은 스택처럼 객체를 자동으로 **해제**하지 않고, 따로 작업이 필요한데 iOS 에서는 **ARC**를 통해 작업을 수행.

- **컴파일 시간 전에 할당**해야 하는 데이터의 양이 너무 **크지 않을 때** **스택**을 사용할 수 있음

  할당해야 할 **데이터 양을 모르**거나 너무 **큰 경우** **힙**을 사용할 수 있음

- **스택**은 **스레드** 별로 다름. 다중 스레드에서는 **각 스레드가 자체 스택**을 가짐

  **힙**은 **애플리케이션** 별로 다름. 

- 전체 프로세스(allocation , reference tracking 및 deallocation) 관점에서 **stack** 이 heap 에 비해 **빠름**.

  allocation 의 경우,  **Stack** allocation은 어셈블리가 단순히 **스택 포인터를 증가**시키면 된다는 것을 의미.

  하지만 **Heap**의 경우, 더 많은 일들이 발생함. memory allocator는 커널에 **빈 페이지(free page)를 요청**하고, 페이지를 적절하게 **분할**해야 하며, fragmentation이 발생할 수 있음. (stack 은 fragmented 될 수 없음)

  deallocation 도 비슷

  근데 사실 "**data access** 할때 힙보다 스택이 빠른가?" 라는 질문은 강조점이 잘못되었음. 

  **동일한 액세스 패턴**을 가진 **동일한 데이터**가 있는 경우 이론적으로 **힙은 스택만큼 빨라**야 함. 예를 들어 데이터가 배열인 경우 **데이터가 연속적**이면 액세스에 **동일한 시간**이 걸림. 하지만 램의 **여러 곳**에 작은 데이터 비트가 **나눠져 있으면** **스택이 더 빠름**.



# 출처

- [Difference between Stack and Heap](https://www.iosiqa.com/2018/10/difference-stack-and-heap-datastructure.html)
- [Stack vs Heap Memory](https://www.educba.com/stack-vs-heap-memory/)
- [Is accessing data in the heap faster than from the stack?](https://stackoverflow.com/questions/24057331/is-accessing-data-in-the-heap-faster-than-from-the-stack)
- [Difference between value type and a reference type in iOS swift?](https://abhimuralidharan.medium.com/difference-between-value-type-and-a-reference-type-in-ios-swift-18cb5145ad7a)

