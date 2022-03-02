# struct 는 왜 상속이 불가능할까?

- Swift **struct 메소드**는 단순 함수 호출일 뿐, **동적으로 디스패치되지 않음**. 

  struct 를 **서브클래싱**하고 메서드를 **override** 하기 위해서는 메서드가 **dynamically dispatched** 되어야함. 이는 구조체에 **vtable(또는 isa 포인터)이 포함**되어야 하는 것을 의미하므로 훨씬 더 무거워짐.

- 적어도 **value type**이 **stack 에 할당**되어, **일정한 공간**을 차지하는 이상 **상속과는 호환(compatible)되지 않**음. 

  **스택**에 할당되는 데이터는 **크기가 정해진 채**로 컴파일 시간에 **메모리에 직접 저장** 됨

  따라서 **subclass** 에 데이터를 추가할 경우, **추가 데이터를 저장할 공간 없**음.



# 출처

- [Why can’t structs inherit from other structs?](https://forums.swift.org/t/why-cant-structs-inherit-from-other-structs/3647)

