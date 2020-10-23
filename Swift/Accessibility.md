# Accessibility
- 앱 cell에 요소가 혼합되어 있으면, 사용자가 각 cell을 **하나의 단위로 사용**할지 또는 cell 내부의 **개별 요소와 상호작용** 하는지 결정해야 한다.
- 전체로 접근 설정할 경우 cell 안의 요소에는 접근하지 못한다.
- 전체로 접근할 경우 `self.isAccessibilityElement = true` 이런식으로 코드에서 설정 (왜냐면 cell의 inspector에는 설정하는게 없다)


## 참고
[iOS ) Accessibility(접근성) - Accessibility Programming Guide for iOS (3)](https://zeddios.tistory.com/460)
