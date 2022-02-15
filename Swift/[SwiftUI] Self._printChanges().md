# [SwiftUI] Self._printChanges()

- **redraw를 트리거**하는 **state property** 를 찾으려는 경우에 유용

- `Self._printChanges()` 는 **private** static method 이기 때문에 따로 documentation 이 없음

- SwiftUI body에 breakpoint 걸고 `po Self._printChanges()` 명령어 입력 가능. 하지만 이렇게 breakpoint를 설정하는 것은 문제의 원인이 되는 View를 알고 있는 경우에만 작동

  예시에서는 `_count` 속성이 변경되었음을 알려줌. count 속성을 상태 값으로 관찰했기 때문에 SwiftUI 뷰가 다시 그려짐
  ![image](https://user-images.githubusercontent.com/20410193/154024285-de398afc-90c8-47ba-acb7-5928aa4e3642.png)


- view의 **body 내부**에서 `Self._printChanges()`를 호출하면 **view 의 업데이트를 촉발한 변경 사항**을 지속적으로 확인 가능 (prints the names of the changed dynamic properties that caused the result of `body` to need to be refreshed)

  ```swift
  var body: some View {
      Self._printChanges()
      return VStack {
          // .. other changes
      }
  }
  
  //console
  TimerCountView: @self, @identity, _count, _animateButton changed.
  TimerCountView: _animateButton changed.
  TimerCountView: _count changed.
  TimerCountView: _count changed.
  ```

- 여기서 `@self` 는 view value 자체가 변경되었음을 의미하고, `@identity` 는 view의 identity 가 변경(즉, 뷰와 관련된 persistent data가 동일한 유형의 새로운 인스턴를 위해 재활용)되었음 을 의미

# 출처

- [Debugging SwiftUI views: what caused that change?](https://www.avanderlee.com/swiftui/debugging-swiftui-views/)
- [[11월 29일] Tech Talks 하이라이트](https://developer.apple.com/kr/news/?id=64t1doqz)
- [Where is Self._printChanges() defined and/or documented for SwiftUI?](https://stackoverflow.com/questions/69859370/where-is-self-printchanges-defined-and-or-documented-for-swiftui)

