# [SwiftUI] Modifying state during view update, this will cause undefined behavior

이 오류는 뷰가 실제로 **렌더링되는 동안 SwiftUI 뷰의 State를 수정**하기 때문에 발생

뷰의 **State를 수정하면 뷰가 다시 렌더링**되어 SwiftUI가 혼란스러워짐. 

문제를 해결하려면 뷰의 State를 변경하는 코드를 뷰 업데이트 외부로 이동

```swift
                .overlay(
                // using geometry reader
                    GeometryReader { proxy -> Color in
                        let minY = proxy.frame(in: .global).minY
                        DispatchQueue.main.async {
                            // 😯 main.async 안에 안넣어주면 아래와 같은 에러
                            // Modifying state during view update, this will cause undefined behavior.
                            self.offset = minY
                        }
                        return Color.clear
                    }
                )
```

# 출처
- https://www.hackingwithswift.com/quick-start/swiftui/how-to-fix-modifying-state-during-view-update-this-will-cause-undefined-behavior
