# UIFeedbackGenerator

- `UIFeedbackGenerator` 덕분에 햅틱 피드백을 앱에 매우 쉽게 통합할 수 있게 됨.

  그러나 해당 클래스는 Feedback Generator의 추상 슈퍼클래스(abstract superclass)기 때문에 직접 사용하진 않고 구체적인 하위 클래스를 생성해서 사용

  1. `UIImpactFeedbackGenerator`
  2. `UISelectionFeedbackGenerator`
  3. `UINotificationFeedbackGenerator`

- 햅틱 피드백은 장치에 내장된 하드웨어인 탭틱 엔진에 의해 생성됨

  전력을 절약하기 위해 탭틱 엔진은 할 일이 없으면 idle 상태이므로, 탭틱 엔진이 깨어나는 데 시간이 걸리며 이로 인해 약간의 지연이 발생할 수 있음

  이 지연을 줄이기 위해 FeedbackGenerator 에 `prepare()` 를 호출해서 탭틱 엔진을 준비하도록 피드백 생성기에 요청할 수 있음. (선택 사항)

- [Adding Haptic Feedback with Feedback Generators in Swift](https://cocoacasts.com/uikit-fundamentals-adding-haptic-feedback-with-feedback-generators-in-swift) 에서 Starter project 를 다운받을 수 있음

  <img width="400" alt="image" src="https://user-images.githubusercontent.com/20410193/173026423-bc6e17ec-2f63-45b4-b38d-04fd4c1493a9.png">


## UISelectionFeedbackGenerator

```swift
        // Initialize Selection Feedback Generator
        let feedbackGenerator = UISelectionFeedbackGenerator()

        // Trigger Haptic Feedback
        feedbackGenerator.selectionChanged()
```

- UISelectionFeedbackGenerator 생성하고 selectionChanged() 호출하면 끝

- 이름에서 알 수 있듯이 이러한 유형의 햅틱 피드백은 선택이 변경되었음을 알리는 데 사용. 

- switch, slider, picker 등의 UI 컴포넌트가 Apple 에 의해 디자인된 햅틱을 사용하는 경우임

- Apple의 Clock 이 해당 햅틱의 좋은 예시로, 선택이 변경될 때마다 애플리케이션은 `UISelectionFeedbackGenerator` 클래스를 사용하여 햅틱 피드백을 생성

  <img width="400" alt="image" src="https://user-images.githubusercontent.com/20410193/173026569-5bb9d8c2-a1e6-47fa-b052-e82974dff619.png">


## UIImpactFeedbackGenerator

```swift
// Initialize Impact Feedback Generator
let feedbackGenerator = UIImpactFeedbackGenerator(style: feedbackStyle)

// Trigger Haptic Feedback
feedbackGenerator.impactOccurred()
```

- `UIImpactFeedbackGenerator` 인스턴스를 생성하고 `impactOccurred()` 호출.

- 이때 `UIImpactFeedbackGenerator` 의 생성자에  `UIImpactFeedbackGenerator.FeedbackStyle` 을 지정할 수 있음.

  - `light`, `medium`, `heavy`  피드백은 무엇을 뜻하는지 명확함. 
  
  - iOS 13 부터  `soft` 와 `rigid` 도 도입했는데, 이건 명확하지도 않고 Apple documentation 도 딱히 도움은 안됨

- `impactOccurred()` 을 호출할 때  햅틱 피드백의 강도를 지정할 수 있도록 CGFloat 값을 넘길 수 있음 ->  `impactOccurred(intensity:)` 

  하지만 불행히도 Apple의 문서에는 어떤 값이 허용되거나 권장되는지 명시되어 있지 않음. `impactOccurred(intensity:)`설명서가 부족하기 때문에 햅틱 피드백을 생성 하는 데 사용하지 않는 것이 좋음

- 만약  `init(feedbackStyle:)` 에서 `light`, `medium` 등으로 피드백 스타일을 지정했다면, `impactOccurred(intensity:)`  으로 전달된 값은 무시되고 `feedbackStyle` 값이 사용됨

## UINotificationFeedbackGenerator

세 번째이자 마지막 `UIFeedbackGenerator`하위 클래스는 `UINotificationFeedbackGenerator`입니다. 이름에서 알 수 있듯이 피드백 생성기는 사용자에게 이벤트를 알리는 데 사용할 수 있습니다. 클

```swift
// Initialize Notification Feedback Generator
let feedbackGenerator = UINotificationFeedbackGenerator()

// Trigger Haptic Feedback
feedbackGenerator.notificationOccurred(feedbackType)
```

- 용자에게 이벤트를 알리는 데 사용할 수 있음
- `UINotificationFeedbackGenerator` 를 만들고 `notificationOccurred(_:)` 를 호출하면 됨
- `notificationOccurred(_:)` 에는  `UINotificationFeedbackGenerator.FeedbackType` 이 들어감

  -  `error`, `success`, `warning` 가 들어갈 수 있음
  
- 애플리케이션이 사용자의 요청을 처리할 수 없는 경우 `error` 피드백 타입이 적합하고,  pull-to-refresh 가 성공적으로 데이터를 불러왔을때는  `success` 가 적합

# 출처

- [Adding Haptic Feedback with Feedback Generators in Swift](https://cocoacasts.com/uikit-fundamentals-adding-haptic-feedback-with-feedback-generators-in-swift) 

- [iOS ) Haptic Feedback](https://zeddios.tistory.com/726)

- [[HIG] User Interaction: Haptics](https://batterflyyin.tistory.com/75)

- [Playing haptics](https://developer.apple.com/design/human-interface-guidelines/patterns/playing-haptics/)

