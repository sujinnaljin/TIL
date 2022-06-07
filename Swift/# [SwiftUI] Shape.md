# [SwiftUI] Shape

view 를 그릴때 사용할 수 있는 2D shape

`func path(in rect: CGRect) -> Path` 를 필수로 구현해야하는 protocol. 

`Shape`는 `path(in:)`에서 주어진 `Rect`를 기반으로 상대 경로를 받아서 그리고, 해당 메서드 호출이 끝나야 최종 적인 사이즈를 알 수 있음. 

명시적으로 fill 또는 stroke가 없는 Shape 는 디폴트로 foreground color 를 기준으로 fill 됨

아래와 같은 Type 들이 Shape 를 conform 함

![스크린샷 2022-06-08 오전 3 12 24](https://user-images.githubusercontent.com/20410193/172453269-24a2ab01-fc76-4863-865b-d20db1963c1e.png)


# 출처

- https://developer.apple.com/documentation/swiftui/shape

- https://seons-dev.tistory.com/entry/SwiftUI-Path
