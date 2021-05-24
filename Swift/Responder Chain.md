# Responder Chain

- **Responde**r는 이벤트를 핸들링하고 반응할 수 있는 객체로서, 이벤트를 받으면 **처리**하거나 다른 responder 객체에 **포워드** 함 

- **Responder chain**은 responder 객체들이 이벤트나 액션 메시지를 핸들링할 책임을 앱의 다른 객체들에게 전송할 수 있도록 해줌

- **이벤트를 포워딩**한다는 것은 responder가 이벤트를 **핸들링하지 않을 경우** 해당 이벤트를 **responder chain**에 따라 **다음 객체로** 보내는 것.

  이 전파(propagation)는 이벤트를 핸들링하는 object가 나올때까지 계속 되고, 마지막까지 처리되지 않을 경우, 앱이 해당 메시지를 버림

- 아래와 같은 뷰 구조에서 (각각 subView로 가지고 있는 형태) 파란색이나 초록색 누르면 responder chain 에 의해 이벤트가 super view로 propagating 되면서 `didTapRed` 가 트리거 됨

![img](https://miro.medium.com/max/670/1*6NehGxxpYYRMHhZur0bKFA.png)

- UIKit은 일정한 규칙을 사용하여 responder chain을 동적으로 관리하는데, 이 규칙에 의해 어떤 객체가 다음 순서로 이벤트를 받을지 결정. 

- 아래 그림에서의 이벤트 전달은 다음과 같음

  `TextField` -> `UIView` -> `윈도우의 루트 뷰` -> `루트 뷰 소유하고 있는 VC` -> `UIWindow` -> `UIApplication`  -> `app delegate` (`UIResponder`의 인스턴스이면서 responder chain의 일부가 아닐 때)



![img](https://miro.medium.com/max/2608/1*XIdBTQ9YCyVjg1cFoxmESQ.png)



- Responder 객체의 `next` 프로퍼티를 오버라이드하여 responder chain 변경 가능. 
- 많은 UIKit 클래스들은 해당 프로퍼티를 오버라이드하여 특정 객체들을 반환
  - `UIView` — 만약 ViewController의 rootView라면, next responder는 ViewController. 아닌 경우엔 해당 view의 superView.
  - `UIViewController` — 만약 ViewController의 view가 window의 rootView라면, next responder는 window 객체. 만약 다른 VC에 의해 프레젠트된 경우, next responder는 presenting VC
  - `UIWindow` — next responder는 `UIApplication`
  - `UIApplication` — next responder는 app delegate. 하지만 앱 딜리게이트가 `UIResponder`의 인스턴스이면서 뷰, 뷰 컨트롤러, 또는 앱 객체 자신이 아닐 때만 해당

# 출처

- [Understanding Responder Chain in UIKit with examples](https://medium.nextlevelswift.com/understanding-responder-chain-in-uikit-with-examples-bb28264defc2)
- [iOS의 Responder와 Responder Chain 이해하기](https://seizze.github.io/2019/11/26/iOS%EC%9D%98-Responder%EC%99%80-Responder-Chain-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0.html)

