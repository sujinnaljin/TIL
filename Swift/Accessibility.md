# Accessibility
- 앱 cell에 요소가 혼합되어 있으면, 사용자가 각 cell을 **하나의 단위로 사용**할지 또는 cell 내부의 **개별 요소와 상호작용** 하는지 결정해야 한다.
- 전체로 접근 설정할 경우 cell 안의 요소에는 접근하지 못한다.
- 전체로 접근할 경우 `self.isAccessibilityElement = true` 이런식으로 코드에서 설정 (왜냐면 cell의 inspector에는 설정하는게 없다)
- 전체 말고 accessibility 를 묶을 경우에는 아래와 같이 UIAccessibilityElement 를 생성하고 묶고자하는 범위를 지정해준다.
```swift
let itemElement = UIAccessibilityElement(accessibilityContainer: self)
itemElement.accessibilityLabel = accessibilityText
       
itemElement.accessibilityFrameInContainerSpace = self.bounds
```
예시는 cell에 대한 접근성 지정인데 `accessibilityFrameInContainerSpace`를 `self.frame` 으로 하면 이상하게 범위가 설정 된다. (생각해보면 x, y 좌표가 0,0 으로 잡히는 boudns 가 맞는거 같다)
그리고 이렇게 만들어진 accessibilityElement 는 다음과 같이 원하는 곳에 할당해준다
```swift
self.accessibilityElements?.append(itemElement)
```
- 별점 등 조작 가능한 것에 적합한 trait 는 `.adjustable` 이다. 그리고 accessibilityIncrement()와 accessibilityDecrement()를 추가 구현해주면 된다

## 참고
- [iOS ) Accessibility(접근성) - Accessibility Programming Guide for iOS (3)](https://zeddios.tistory.com/460)
- [adjustable](https://developer.apple.com/documentation/uikit/uiaccessibility/uiaccessibilitytraits/1620177-adjustable)
