# Dependency Injection (feat. Dependency Inversion)

- 아래와 같이 객체가 의존성을 스스로 생성하고 관리하지 않고, 우리가 객체에 의존성을 주입시켜주는 것
> 종속성 주입은 개체에 인스턴스 변수를 제공하는 것을 의미합니다. 정말. 그게 다야. 
> 
> (Dependency injection means giving an object its instance variables. Really. That’s it)

```swift
//스스로 객체를 생성해서 사용 -> DI (X)
class LibraryViewModel {
     var library = Library()
 }
```

## 장점

1. 객체간 사이의 의존성을 최소화 해서 Coupling을 줄이고, 재사용성을 높임
2. 테스트 용이
3. 코드 단순화

## 방법

### 1. Constructor Injection

- constructor나 initializer를 통해 의존성 주입
- 객체를 생성할때 모든 의존성을 주입하기 때문에, 객체에 필요한 속성 일부를 까먹고 안넣어주는 경우를 방지
- 초기화 단계에서 전달한 의존성들은 불변적으로 만들어짐

```swift
class LibraryViewModel { 
    var library: Library 

    init(library: Library) {  
        self.library = library  
    }
}
```

### 2. Property Injection

- 의존성이 대체되거나 수정될 수 있다는 허점이 있음.  즉 의존성이 불변적인 것이 아님
- 또한 해당 Property를 사용할때, 의존성 주입이 되었는지 확인하고 써야함

```swift
class LibraryViewModel {
     var library = Library?
}
let libraryViewModel = LibraryViewModel()  
let library = Library()  
libraryViewModel.library = library
```

### 3. Method Injection (Parameter-based Injection)

- 만약 의존성이 딱 한번 필요한거라면, 객체 안에 property로 저장할 필요가 없고, 그냥 Method의 Prameter로 넘겨주는 형태로 사용할 수 있음
- method가 불릴때마다 의존성이 달라지고, 해당 reference를 유지하지 않기 때문에 local method scope에서만 사용 가능

```swift
protocol LibraryManager { 
    func deleteAllBooks(in library: Library)  
}
```



## 의존관계 역전 (Dependency Inversion)

- 의존성을 주입하는 것만으로 DI(의존성 주입) 이라고 하지는 않음.

- **의존관계 역전 원칙**에 따라 의존관계를 분리해야 함

  > 의존 관계 역전 원칙
  >
  > - 상위 계층(정책 결정)이 하위 계층(세부 사항)에 의존하는 전통적인 의존관계를 반전(역전)시킴으로써 상위 계층이 하위 계층의 구현으로부터 독립되게 할 수 있다. 이 원칙은 다음과 같은 내용을 담고 있다
  >
  >   첫째, 상위 모듈은 하위 모듈에 의존해서는 안된다. 상위 모듈과 하위 모듈 모두 추상화에 의존해야 한다.
  >
  >   둘째, 추상화는 세부 사항에 의존해서는 안된다. 세부사항이 추상화에 의존해야 한다.
  >
  > - 이 원칙은 '상위와 하위 객체 모두가 동일한 추상화에 의존해야 한다'는 객체 지향적 설계의 대원칙을 제공

- iOS에서는 protocol을 이용

![Dependency inversion.png](https://upload.wikimedia.org/wikipedia/commons/9/96/Dependency_inversion.png)

```swift
//Figure 1
class A {
  //B class와 의존관계가 있음
  var variable: B
  init(variable: B) {
    self.variable = variable
  }
}
class B { }

//Figure 2
//의존 관계를 독립시킬 인터페이스
protocol DependencyIndependentInterface: class { }
class A {
  var variable: DependencyIndependentInterface
  init(variable: DependencyIndependentInterface) {
    self.variable = variable
  }
}
class B: DependencyIndependentInterface { }
```

- "의존의 방향이 역전 되었다", "의존의 전이를 끊었다" 등으로 표현되는 **제어의 주제가 반전(역전) 되는 패턴**을 **IOC (Inversion Of Control)** 라고 함
-  제어 반전의 목적은 다음과 같음
   - 작업을 구현하는 방식과 작업 수행 자체를 분리
   - 모듈을 제작할 때, 모듈과 외부 프로그램의 결합에 대해 고민할 필요 없이 모듈의 목적에 집중
   - 다른 시스템이 어떻게 동작할지에 대해 고민할 필요 없이, 미리 정해진 협약대로만 동작하게 하면 됨
   - 모듈을 바꾸어도 다른 시스템에 부작용 없음

## IOC Container

- 위의 IOC를 구현하는 프레임워크
- 제어권을 컨테이너(프레임워크)가 가져가는 것
- 컨테이너(프레임워크)로 객체를 관리하고 객체의 생성을 책임지고, 의존성을 관리

# 출처

- [Dependency Injection in Swift](https://kimgaeun.tistory.com/2)

- [Dependency Injection in Swift](https://github.com/JeongHoonkr/Learning-Swift/blob/master/Study/English/Dependency_Injection_in_Swift.md)

- [[DI] Dependency Injection 이란?](https://medium.com/@jang.wangsu/di-dependency-injection-%EC%9D%B4%EB%9E%80-1b12fdefec4f)

- [의존관계 역전 원칙](https://ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4%EA%B4%80%EA%B3%84_%EC%97%AD%EC%A0%84_%EC%9B%90%EC%B9%99)

- [의존성 역전과 의존성 주입은 별 상관이 없지 않나요? 답글](https://medium.com/@jang.wangsu/%EA%B0%9C%EC%9D%B8%EC%A0%81%EC%9C%BC%EB%A1%9C%EB%8A%94-161a895f14d3)
- [10 Tricks To Avoid Spaghetti Code in iOS Development](https://betterprogramming.pub/10-tricks-to-avoid-spaghetti-code-in-ios-development-3f5a0ab2f46f)

  

