# [SwiftUI] Animations

- 애니메이션을 구현하려면 변경되는 뷰가 있어야 함. 
- 모든 뷰는 2가지 상태를 가지고 있음. 하나는 **시작** 상태이고, 다른 하나는 **종료** 상태이며, **상태 간의 전환을 애니메이션**이라고 함
![image](https://user-images.githubusercontent.com/20410193/128696887-60128705-975e-4cda-919e-907842ddb494.png)



- SwiftUI에서는 위치, 크기, 색상, 회전 등과 같이 숫자 속성이 있는 모든 항목을 애니메이션으로 만들 수 있음
- SwiftUI에는 명시적 및 암시적 두 가지 유형의 애니메이션 존재

## 명시적 애니메이션

**`withAnimation { … }`** 클로저 로 정의되며 **중괄호 안에서 변경된 매개변수 값**만 **애니메이션 적용** 됨



## 암시적 애니메이션

- SwiftUI에서 가장 간단한 종류의 애니메이션
- 내장된 **`animation()` modifier**을 통해 애니메이션 지원
- modifier는 **뷰 내에서 발생하는 모든 변경**에 사용 됨
- 해당 modifier를 사용하려면 **view에 대한 다른 modifier의 뒤에 배치**하고 원하는 애니메이션 종류를 지정

### 기본 애니메이션

- SwiftUI에는 기본적으로 사용할 수 있는 기본 애니메이션 curve가 많음

  - **linear:** 애니메이션이 지정된 기간 동안 **일정한 속도**로 수행됨
  - **easyOut:** 애니메이션이 **빠르게 시작**되고 시퀀스의 끝이 가까워지면 **느려짐**
  - **easyIn:** 애니메이션 시퀀스는 **느리게 시작**하여 끝이 가까워지면 **빨라짐**
  - **easyInOut:** 애니메이션이 **느리게 시작**되었다가 **빨라졌**다가 **다시 느려짐**

  여기서 모든 애니메이션에는 `duration` 속성 존재 -> 애니메이션의 기본 지속 시간을 변경하려는 경우 사용

![image](https://user-images.githubusercontent.com/20410193/128696898-c2a0bdd4-c4b5-4d7f-a3e6-2f39c489492a.png)



# 출처

- [Basics of SwiftUI Animations](https://medium.com/simform-engineering/basics-of-swift-ui-animations-d1aa2485a5d9)

