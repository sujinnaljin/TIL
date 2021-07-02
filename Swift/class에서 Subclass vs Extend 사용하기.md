# class에서 Subclass vs Extend 사용하기

## Extension

- 클래스의 **모든 인스턴스에서 사용**할 수 있어야하는 기능을 추가하는 경우 **extension** 사용
- 기존 class에 추가만 가능
- stored property 속성 가질 수 없고, **computed var이나 func만 추가** 가능
- 하지만 extension은 **global** 하기 때문에 **extend한 class의 모든 instance**에서 사용 가능

## Subclass

- 클래스의 **일부 인스턴스에만 적용되는 기능**을 만들려면 **subclass** 사용
- **let이나 var 등 새 속성**을 추가하고, **함수 override** 가능
- 기능을 사용하기 위해서는 **새로운 class의 instance를 생성**해야함 (새로운 속성, 기능은 superclass의 일반 instance에서는 사용 불가)
- Subclass는 별도의 이름을 부여 할 수 있다는 점에서도 훌륭



# 출처

- [Subclass or Extend classes in Swift](https://stevenpcurtis.medium.com/subclass-or-extend-classes-in-swift-fe51025be27b)

