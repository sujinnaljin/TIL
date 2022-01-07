# modalPresentationStyle

- 프레젠테이션 스타일은 시스템이 **모달 뷰 컨트롤러를 표시하는 방법**을 정의

  ```swift
  let redViewController = UIStoryboard(name: "Main", bundle: nil)
                          .instantiateViewController(identifier: "redViewController")
  redViewController.modalPresentationStyle = .fullScreen
  self.present(redViewController, animated: true)
  ```

- 시스템은 이 값을 **regular-width size class** 에서만 사용

- **compact-width size class** 에서 **일부 스타일은 다른 스타일의 동작**을 취함

- 프레젠테이션 스타일은 모달 뷰 컨트롤러의 **콘텐츠 크기**에도 영향을 미침

  - 예를 들어 [`UIModalPresentationStyle.pageSheet`](https://developer.apple.com/documentation/uikit/uimodalpresentationstyle/pagesheet) 은 시스템에서 제공하는 명시적 크기를 사용
  - 반면  [`UIModalPresentationStyle.formSheet`](https://developer.apple.com/documentation/uikit/uimodalpresentationstyle/formsheet) 은 사용자가 설정할 수 있는 뷰 컨트롤러의 [`preferredContentSize`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621476-preferredcontentsize)  속성을 사용

- default 는 [`UIModalPresentationStyle.automatic`](https://developer.apple.com/documentation/uikit/uimodalpresentationstyle/automatic) 

## UIModalPresentationStyle

뷰 컨트롤러를 표시할 때 사용할 수 있는 **presentation 스타일**들은 아래와 같음

- automatic

  - 시스템에서 선택한 **기본** 프레젠테이션 스타일

- none

  - 적용하지 않음을 나타내는 프레젠테이션 스타일 (indicates no adaptations should be made)

- fullScreen

  - view가 화면을 덮는 프레젠테이션 스타일

  - 무조건 화면 전체를 차지하도록 띄워서 새로 띄워진 뷰 아래 깔리는 뷰들은 전혀 보이지 않게 됨. 
  - Presentation의 기본값이고, 다른 PresentationStyle이 자신의 스타일을 적용할 수 없는 상황에서는 FullScreen으로 동작

- pageSheet

  - **하단 content을 부분적으로 가리는** 프레젠테이션 스타일

  - regular-width, regular-height (보통 아이패드 가로 / 세로 모드) 에서 컨텐츠는 heigth가 width보다 큰 페이지 크기로, 실제 치수는 디바이스의 화면 크기와 방향을 포함한 여러 요소에 따라 달라짐.

    ![image](https://user-images.githubusercontent.com/20410193/148471022-15a37e5f-4123-444e-9361-d35533827ad8.png)

  - compact-width, regular-height (보통 iphone 세로 모드) 에서 시스템은 sheet로 뷰 컨트롤러를 표시

  - compact-height (보통 iphone 가로 모드) 일때는 FullScreen으로 동작

  - custom 컨텐츠 크기를 제공하려면 formstyle 을 사용하고 모달 뷰 컨트롤러의 기본 ContentSize 속성을 설정

- formSheet

  - **화면 중앙**에 내용을 표시하는 프레젠테이션 스타일

  - regular-width, regular-height  (보통 아이패드 가로 / 세로 모드) 에서는 dimming layer 위에 뷰 컨트롤러의 컨텐츠를 배치. 기본 content 크기는 pageSheet 스타일 크기보다 작음.

  - compact-width, regular-height (보통 iphone 세로 모드) 에서 시스템은 sheet로 뷰 컨트롤러를 표시

  - compact-height (보통 iphone 가로 모드) 일때는 FullScreen으로 동작

    ![image](https://user-images.githubusercontent.com/20410193/148471041-0c3d088c-bf67-4287-8c2e-63183fa80b0b.png)

- currentContext

  - 다른 뷰컨트롤러의 컨텐츠에 컨텐츠가 표시되는 프리젠테이션 스타일
  - 글자 그대로 해석하면 현재 문맥에 맞춰서 띄우라는 뜻으로, 정확히는 현재 뷰컨트롤러를 시작으로 해서 뷰컨트롤러의 [definesPresentationContext](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621456-definespresentationcontext)가 true로 설정되어 있는 뷰컨트롤러의 영역에 맞춰서 새로운 뷰가 띄워짐. 일반적인 뷰컨트롤러는 기본값이 false지만, navigationController 등은 이 값이 true로 되어 있음. 해당 프로퍼티가 true인 뷰컨트롤러가 없는 경우, FullScreen으로 동작.
  - 이는 화면에 2개 이상의 뷰 컨트롤러가 영역을 차지하고 있는 경우(SplitViewController 등) 하나만 바꾸고 싶다던가 할 때에 유용하게 사용할 수 있음
    ![image](https://user-images.githubusercontent.com/20410193/148471059-9bddc182-7290-43d9-ba82-528190964ba5.png)

- custom

  -  custom presentation controller 및 하나 이상의 custom animator object에 의해 관리되는 custom view presentation style 

- overFullScreen

  - 뷰가 화면을 덮는 보기 프레젠테이션 스타일
  - FullScreen과 동일하지만 띄워지는 뷰가 투명할 경우 아래 쌓인 뷰가 비쳐보임. FullScreen에서는 아래 깔리는 뷰가 뷰 계층에서 지워지지만 overFullScreen을 적용하면 뷰 계층에서 사라지지 않음

- overCurrentContext

  - 다른 뷰 컨트롤러의 컨텐츠에 컨텐츠이 표시되는 프리젠테이션 스타일
  - currentContext와 동일하지만 띄워지는 뷰가 투명할 경우 아래 쌓인 뷰가 비쳐보임. CurrentContext에서는 아래 깔리는 뷰가 뷰 계층에서 지워지지만 overCurrentContext를 적용하면 뷰 계층에서 사라지지 않음

- popover

  - 컨텐츠이 popover view에 표시되는 프리젠테이션 스타일
  - Popover는 다른 스타일과는 많이 다른데, 뷰의 특정 위치에서 팝업창처럼 뜨는 형태이기 때문에 두가지 옵션을 필수적으로 신경써야 함

    1. 어느 정도의 크기로 띄워줄 것인가? - [preferredContentSize](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621476-preferredcontentsize) 프로퍼티로 조정. 이 값은 Popover로 뜨는 뷰컨트롤러에 설정.

    2. 어떤 뷰를 중심으로 띄울 것인가? - Popover되는 뷰컨트롤러의 popoverPresentationController 프로퍼티를 통해 설정. 두 가지 방법 중 반드시 하나만 적용되어야 함

       1. [barButtonItem](https://developer.apple.com/documentation/uikit/uipopoverpresentationcontroller/1622314-barbuttonitem) 프로퍼티를 설정
       2. [sourceView](https://developer.apple.com/documentation/uikit/uipopoverpresentationcontroller/1622313-sourceview)와 [sourceRect](https://developer.apple.com/documentation/uikit/uipopoverpresentationcontroller/1622324-sourcerect) 프로퍼티를 설

  - Popover는 compact width 인 경우에는 full screen 으로 동작함. 기본적으로 regular width 인 경우에만 적용됨.

  - Popover된 뷰 컨트롤러의 바깥 영역을 터치하게 되면 Popover된 뷰컨트롤러는 자동으로 dismiss 됨. 하지만  compact width 일 경우에 Popover가 작동할 경우에는 FullScreen이 되기 때문에 이 경우에도 dismiss가 제대로 될 수 있도록 장치를 마련해 둘 필요가 있음.

- blurOverFullScreen

  - full screen presentation에 새 콘텐츠를 표시하기 전에 기본 콘텐츠를 흐리게 하는 프레젠테이션 스타일

# 출처

- [modalPresentationStyle](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621355-modalpresentationstyle)
- [ModalPresentationStyle, TransitionStyle 파헤치기](https://jcsoohwancho.github.io/2019-07-31-ModalPresentationStyle,-TransitionStyle-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0/)
- [iOS Page sheet & Form sheet](https://stackoverflow.com/questions/38584411/ios-page-sheet-form-sheet)
- [UIModalPresentationStyle.pageSheet](https://developer.apple.com/documentation/uikit/uimodalpresentationstyle/pagesheet)
- [Adaptivity and Layout](https://stackoverflow.com/questions/27287096/ios-difference-in-using-compact-regular-and-compact-any-size-classes)

