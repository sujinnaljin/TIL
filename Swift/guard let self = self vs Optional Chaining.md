# guard let self = self vs Optional Chaining

- `[weak self]` 를 사용할 때 `self?` 대신 `guard let self = self` 를 사용하면 경우에 따라 **deallocation 을 지연**시킬 수 있다. (의도에 따라 좋거나 나쁠 수 있음)
- closure 시작 할때 `guardlet self = self {return}` 를 사용하는 것은 **값비싼 직렬 작업**(serial work) 또는 **세마포어와 같은 스레드 차단 메커니즘** 때문에 deallocation을 지연 시킬 수 있음
- `guard let` 은 `self` 가  `nil` 인지 체크하고, 만약 값이 있다면 일시적으로 **scope 동안 `self` 에 대한 강한 참조** 유지. 즉, closure 내에서 self 가 무조건 유지 됨
- 만약 `guard let` 대신 **`self?` 를 사용한다면 매번 호출시 마다 nil 체크**를 하기 때문에 closure 의 어느 시점에서든 self 가 nil 이 되면 해당 라인을 무시하고 다음 라인으로 건너뜀 -> Optional chaining 은 **deallocation 지연 야기 하지 않음**
- 다소 미묘한 차이이지만, **`self?`** 를 통해 **뷰 컨트롤러를 해제한 후 불필요한 작업을 피하**거나, 반대로 **`guard let`** 을 통해 객체가 **deallocated 되기 전에 모든 작업을 완료**하려는 경우 (예: 데이터 손상을 방지하기 위해)에 대해서 이 차이를 고려해야함



# 출처

- [You don’t (always) need [weak self]](https://medium.com/flawless-app-stories/you-dont-always-need-weak-self-a778bec505ef)

