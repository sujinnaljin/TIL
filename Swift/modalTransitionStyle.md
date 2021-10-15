# modalTransitionStyle

- 뷰 컨트롤러를 표시할 때 사용할 **전환 스타일**

- [`present(_:animated:completion:)`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621380-present) method 를 사용해서  뷰 컨트롤러가 표시될 때 화면에 애니메이션되는 방식을 결정

- transition type을 변경하려면 뷰 컨트롤러를 표시하기 전에 이 속성을 설정해야 함

  ```swift
  func openRedViewController() {
    let redViewController = UIStoryboard(name: "Main", bundle: nil)
                            .instantiateViewController(identifier: "redViewController")
    redViewController.modalPresentationStyle = .fullScreen
    redViewController.modalTransitionStyle = .partialCurl
    self.present(redViewController, animated: true)
  }
  ```

- 이 속성의 기본값은 coverVertical

## UIModalTransitionStyle

뷰 컨트롤러를 표시할 때 사용할 수 있는 **전환 스타일**들은 아래와 같음

- coverVertical
  - **default** transition style
  - 뷰 컨트롤러가 **present** 되면 뷰 컨트롤러가 **화면 아래쪽에서 위로** 올라오고, **dismiss** 되면 뷰가 다시 **아래로** 내려감. 

- flipHorizontal
  - 가로방향으로 가운데를 축으로 삼아 마치 **카드가 뒤집하는 듯**한 효과
  - 뷰 컨트롤러가 **present** 되면 현재 view가 오른쪽에서 왼쪽으로 수평 3D flip을 시작하여 **새 view가 이전 view의 뒷면에 있던 것처럼** 표시. **dismiss** 되면 flip은 왼쪽에서 오른쪽으로 진행되어 **원래 view**로 돌아감.

- crossDissolve
  - 화면 **교차**되는 효과
  - 뷰 컨트롤러가 **present** 되면 현재 view가 **fade out** 되고, 새로운 view가 **fade in** 됨. **dismiss** 되면 유사한 **cross-fade**로 **원래 view**로 돌아감 
- partialCurl
  - **종이를 넘기는 듯**한 효과
  - 뷰 컨트롤러가 **present** 되면 현재 view의 **한쪽 모서리가 curl up** 되면서 아래 있던 view가 나타남. **dismiss** 되면 **curl up 되었던 페이지가 다시 view 위**에 펼쳐짐.
  - 이 transition을 사용하여 표시되는 뷰 컨트롤러는 **추가 뷰 컨트롤러를 표시할 수 없**음. (A view controller presented using this transition is itself prevented from presenting any additional view controllers)
  - 이 transition style은 **parent 뷰 컨트롤러가 full-screen 으로 표시**되고 있고, **넘길때 modalPresentationStyle 도 fullScreen** 으로 지정해야함. 그 외에는 exception 트리거 됨.



# 출처

- [iOS — Custom Transitions](https://medium.com/@adi.mizrahi/ios-custom-transitions-7cbbfbc9d389)
- [modalTransitionStyle](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621388-modaltransitionstyle)
- [modalPresentationStyle](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621355-modalpresentationstyle)
- [ModalPresentationStyle, TransitionStyle 파헤치기](https://jcsoohwancho.github.io/2019-07-31-ModalPresentationStyle,-TransitionStyle-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0/)

