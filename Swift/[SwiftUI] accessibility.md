# [SwiftUI] accessibility.md

- `.accessibility(hidden: true)`

  아예 element 접근성 감춰버림

- `.accessibilityElement(children: .ignore)` 

  VoiceOver에 영역이 잡히긴 하지만 읽히진 않음. (그냥 `.accessibilityElement()` 랑 비슷한거 같은디요?)

- `.accessibilityElement(children: .combine)`

  상위뷰에 적용해서  하위 뷰를 단일 접근성 요소로 결합하도록 요청. 한번에 문장으로 읽도록 설계되지 않았기 때문에 두 텍스트 사이에 일시 중지가 추가됨

- `.accessibilityElement(children: .contain)` 

  child 접근성 요소는 새 접근성 요소의 children 이 됨. 상위 접근성 문구 먼저 읽고 하위 뷰 문구 이어 읽음
  ![image](https://user-images.githubusercontent.com/20410193/128598065-85c32ae2-c8de-4404-b92c-a85e97f63a45.png)

  만약 `VStack{}.accessibilityElement()` 으로 `Seconds Elasped,` `Seconds Remaing` 각각 잡아서 라벨 설정하고 상위 `HStack.accessibilityElement(children: .contain)` 적용하면, HStack 접근성 문구 먼저 읽고 VStack도 문구 이어서 읽음

  그리고 각 VStack 에 대한 접근성이 잡히긴 하지만 보이스로 읽어주는건 다른 영역에서 HStack  진입할때임. 

  즉 `프로그레스 →  Seconds Elasped → Seconds Remaing → Speaker 1 of 3` 

  프로그레스 접근, 문구 읽음 → Seconds Reamaining 접근, HStack 접근성 문구 먼저 읽고 VStack도 문구 있으면 읽음 → Seconds Reamaining 접근, 하지만 접근성 문구 안읽음 → Speaker 1 of 3 접근, 문구 읽음

  반대로 `Speaker 1 of 3 → Seconds Remaing → Seconds Elasped → 프로그레스` 라면 `Seconds Remaing` 접근할때는 문구 읽고, `Seconds Elasped` 접근할 땐 문구 안읽음

  만약 상단 HStack에서 `.accessibilityElement(children: .contain)` 안쓰고 라벨만 설정하면 HStack 의 라벨만 두 영역 모두 읽고 하위 VStack의 label 은 읽지 않음


- `accessibilityLabel`

  접근성 문구.

  button의 경우 image 가 있다면 해당 image 이름으로 label 이 설정되기 때문에 따로 설정이 필요할 수 있음.

- `accessibilityValue`

  state 가지고 있는 것에 대해 설정.

  Ex) 슬라이더 50%로 설정되어있으면 “50%”로 설정

- 아래 코드에서 VoiceOver는 systemImage 값을 읽으므로 element를 설명하는 접근성 설정해야함

  ```swift
  Button(action: {}) {
          Label("Skip song", systemImage: "forward")
      }
      .accessibilityLabel(Text("Skip song"))
  ```

