# SpriteView

- SpriteKit scene 을 렌더링하는 SwiftUI 뷰

  ```swift
  SpriteView(scene: RainFall(),
             options: [.allowsTransparency])
  ```

- options 에 `SpriteView.Options` 값을 넣을 수 있는데 아래와 같은 타입 프로퍼티가 있음

  - [`static let allowsTransparency: SpriteView.Options`](https://developer.apple.com/documentation/spritekit/spriteview/options/3592878-allowstransparency)

  - [`static let ignoresSiblingOrder: SpriteView.Options`](https://developer.apple.com/documentation/spritekit/spriteview/options/3592884-ignoressiblingorder)

  - [`static let shouldCullNonVisibleNodes: SpriteView.Options`](https://developer.apple.com/documentation/spritekit/spriteview/options/3592910-shouldcullnonvisiblenodes)

- 그 중 allowsTransparency 를 주면 씬의 background 를 투명하게 설정 가능

  *allowsTransparency 안주고 scene 의 backgroundColor = .claer 설정 했을때*

  ![image](https://user-images.githubusercontent.com/20410193/173630838-e54ebdf4-e066-4605-b57d-8843e2080821.png)
  
   *allowsTransparency 주고 scene 의 backgroundColor = .claer 설정 했을때*

  ![image](https://user-images.githubusercontent.com/20410193/173630922-45cd3870-aab1-4e66-a23c-c672270a4819.png)


# 출처

- https://developer.apple.com/documentation/spritekit/spriteview
- https://developer.apple.com/documentation/spritekit/spriteview/options
