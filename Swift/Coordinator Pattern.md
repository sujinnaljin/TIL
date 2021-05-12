# Coordinator Pattern

- **화면 간의 연결에 대한 의존성 분리**를 도와주는 패턴

- 기존에는 VC에서 화면 전환이 발생할때, 화면 관리에 대한 추적이 힘든 이슈 있음.

- 새로운 VC가 어디서 생성되었는지 (MVVM의 경우, 언제 ViewModel 도 주입되었는지를) 추적하려면 선언부를 찾아가야하는데, 점점 해당 VC를 사용하는 곳이 많아지면 찾기가 힘들어 지기 때문

- 따라서 화면의 관리를 해주는 Coordinator를 만들어, 화면간의 이동과 생성에 대한 의존성을 위임 -> Coordinator Pattern

- 기본적으로 코디네이터는 다음의 기능을 수행

  - ViewController, ViewModel, Reactor .. 등의 화면단위에 사용하는 클래스를 **인스턴스화**
  - ViewController 및 ViewModel, Reactor .. 등에 **종속성**을 주입해야될 인스턴스에 **삽입**
  - ViewController를 **화면에 표시하거나 Push**

- VC끼리는 느슨한 결합을 할 수 있고, 따라서 코드의 재사용성도 높아짐

  

- 각각의 화면마다 **Coordinator**를 두었고, **Coordinator**들은 **ViewModel**을 참조하고 있어, **ViewModel**에서 발생한 유저의 이벤트에 따라 적절한 화면으로 이동

# 출처

- [MVVM 그리고 Coordinator](https://medium.com/nbt-tech/mvvm-%EA%B7%B8%EB%A6%AC%EA%B3%A0-coordinator-a8d4f9d0fc8a)
- [[디자인 패턴] Coordinator pattern](https://medium.com/@jang.wangsu/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-swift-coordinator-pattern-426a7628e2f4)

