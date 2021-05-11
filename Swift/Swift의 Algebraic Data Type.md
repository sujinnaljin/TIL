# Swift의 Algebraic Data Type

Swift와 같이 statically typed 프로그래밍 언어를 사용할 때 두 가지 다른 방식으로 type을 생각할 수 있다. 

## 1. 메모리 레이아웃 관점

- 메모리 레이아웃 관점에서 type은 프로그램의 런타임이 메모리의 원시 바이트를 해석하는 규칙
- 프로그램이 "ㅇㅋㅇㅋ 여기서 120 바이트는 `Person` type인데, 처음 4 바이트는에 `age` 에 할당되고 나머지 116 바이트는 `name` 에 할당하면 되는군" 생각하는 것
- 이는 메모리 안전을 제공하고 메모리 손상을 방지하는 데 도움

![img](https://miro.medium.com/max/3176/1*h17d7rNqM7su-vnlO9uv2g.png)

## 2. Set Algebra 관점

- 보유 할 수 있는 값의 집합 
- 즉, Set Algebra 관점에서 type은 간단히 말해서 값의 집합

![img](https://miro.medium.com/max/1908/1*cjUE-Djltsow9GsXVplwTQ.jpeg)

- Algebra data type 은 기본적으로 Composite type. 그 중에서 2가지 type으로 나뉨
  - Product type
  - Sum type

> Primitive type: 일반적으로 더 작은 유형으로 나눌 수 없는 type. 단순하고 "atomic"
>
> Composite type: Primitive 이나 Composite type 이 결합된 형태

### 1. Algebra data type - Product type

- Product type 은 AND 접속사를 이용해 primitive type을 결합

- 예를 들어 person 이 `age` 와 `isMarried` 속성이 있을 수 있다. 

- Swift에서 product type은 Tuple, Struct, Class 등으로 표현됨

  ![img](https://miro.medium.com/max/1894/1*7YnFIOJSJAwgfDm8Zit2qA.jpeg)

- `Person` type이 보유 할 수있는 모든 가능한 값을 계산하려면 `isMarried`필드와 `age`필드 의 값 수를 곱한다.

  ![img](https://miro.medium.com/max/1872/1*5Uw1gecrBU5AZ49sCqQgVg.jpeg)

  ![img](https://miro.medium.com/max/1920/1*FcGcO2er8I5PP7-GiXl0ig.jpeg)

### 2. Algebra data type - Sum type

- Sum type 은 OR 접속사를 이용해 primitive type을 결합

- 선택을 나타냄

- Swift에서 enumeration으로 표현

  ![img](https://miro.medium.com/max/1528/1*LfrUC5BPNuwekyYnO943_w.jpeg)

- `Document` 는 출생 증명서 **또는** 운전 면허증 일 수 있음. 동시에 둘 다일 수 없음 -> 상호 배타적

- `Document` 유형 의 가능한 모든 값을 계산하려면 각 케이스의 값을 더한다

  ![img](https://miro.medium.com/max/1930/1*tZaBZzx4kXimsQv6ee9qFg.jpeg)

  ![img](https://miro.medium.com/max/1938/1*8Oq3zyJaz9RxItbptlkHFA.jpeg)

### 실제 사례

- 물론 Sum과 Product 유형 결합 가능

  -> 구조체는 내부에 enum 값을 가질 수 있고 enum은 내부에 구조체 값을 가질 수 있다

# 출처

- [Algebraic Data Types in Swift](https://medium.com/nerd-for-tech/algebraic-data-types-in-swift-2a777b24253d)

