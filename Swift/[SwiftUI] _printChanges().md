# [SwiftUI] _printChanges() 

- `_printChanges` 는 **뷰 자체를 다시 로드하게 만든 변경 사항을 구별**하는 데 사용할 수 있는 SwiftUI의 internal API 메서드 (디버그 용)
- 즉 이 함수를 통해 **어떤 데이터 변경으로 인해  view가 업데이트 되는지 파악**할 수 있음
- `_printChanges()` 가 **static method** 이기 때문에 앞에 `Self.` 를 붙여 호출해야함
- **`body` 내에서 호출** 필요. 따라서 일반 **view 코드에는 명시적 return 키워드를 추가**해야 함을 의미
- 기본적으로 모든 view에는 `_printChanges()` 함수가 있음
- 밑줄( `_`)로 시작하는 함수 및 속성 은 **비공개 API**의 일부이므로 이에 대한 문서는 표시되지 않음.
- 이 API를 사용하려면 앱이 **iOS 15 이상**에 있어야함
- `print()` 를 이용해 `State ` 속성을 출력하면  SwiftUI가 view를 rerender 하므로, 디버깅을 위해서는 `_printChanges()` 를 대신 사용

- 아래 코드에서 버튼을 탭하기 시작하면 콘솔에 무언가가 표시되는데, 이는 **특정 속성에 변경 사항이 발생**했음을 의미. 그리고 **속성 변경이 UI와 관련**이 있는 것이기 때문에 SwiftUI는 view를 재렌더링함.

```swift
struct PrintingChanges: View {
    @State var counter = 0
    
    var body: some View {
        Self._printChanges()
        return Button {
            counter += 1
        } label: {
            Text("Current Value \(counter)")
        }   
    }
}
```

![img](https://miro.medium.com/max/1400/1*NIlYyj4231eAEP35-HsWSg.png)



# 출처

- [Two SwiftUI Debugging Helpers You Can Start Using Right Away](https://betterprogramming.pub/two-swiftui-debugging-helpers-you-can-start-using-right-away-26d25630c0cf)

