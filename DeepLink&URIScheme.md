# DeepLink와 URI 스킴

- `딥 링크` 는 사용자가 앱 내부 콘텐츠에 직접 도달하도록 해주는 모든 링크
- `딥 링크` 중 하나의 기술이 `URI 스킴` 으로, 앱 스키마 커스텀 설정 후 사용할 수 있다
- `URI 스킴`에는 앱이 설치 안되어 있는 경우 에러가 뜨고, 다른 스킴과 겹쳐 충돌이 일어날 수 있음
- 그래서 iOS와 android에서는 요즘 `유니버셜 링크`와 `앱 링크`를 주로 활용함

## 딥 링크란?

- **사용자가 앱 내부 콘텐츠에 직접 도달하도록 해주는 모든 링크를 의미한다.** (ex.  http://example.com/my-awesome-content-page)
- **유니버설 링크**, **URI 스킴** 및 **앱 링크**는 **모바일 딥 링크가 작동하도록 하는 기술**들이다.

## 스키마란?

- **외부에서 자신의 앱에 접근**할 수 있도록 해주는 하나의 **통로**이다. 
- **URI 스킴을 활용**한 것으로, 우리가 평소에 웹 사이트 주소로 활용하는 URL(URI 하위개념)와 비슷한 활용이라고 보면 된다.

## URI 스킴이란?

- URI는 인터넷에 있는 자원을 나타내는 유일한 주소로, 인터넷 프로토콜에 항상 붙어 다닌다. 

- 스킴은 프로토콜을 지정하는 부분을 의미한다.

  ```
  scheme:[//[user[:password]@]host[:port]][/path][?query][#fragment]
  ```

  우리가 웹사이트 주소로 자주 활용하는 HTTP인 경우에는 웹 브라우저를 이용한다는 것.

- 웹사이트를 통해 앱을 실행시킬 수 있도록, 사전에 **URI 스킴을 만들고 애플리케이션에서 스키마 설정** 해줄 수 있다.

  ```
  # 페이스북과 카카오의 커스텀 스키마
  fb://
  kakaotalk://
  ```

- iOS의경우 Info.plist의 URL Types에서 **Identifier에 bundle id**를 지정해주고 **URL Schemes를 추가**해준다. 
  여기에 정의된 URL scheme은 앞으로 커스텀 스키마로 딥링크를 호출 할때 **swiftexamples://** 라는 식으로 사용된다.

  ```swift 
  func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
        
      if let scheme = url.scheme {
          if scheme == "swiftexamples" {
              let uvc = initStoryboard("Main", "main") as! MainViewController
              window.rootViewController = uvc
          }
      }
      return true
  }
  ```

  Delegate에 위와 같은 코드를 추가하고 사파리에서 **swiftexamples://** 라고 입력해보자.  open을 누르면 우리 앱으로 바로 가지는 것을 확인할 수 있다.
  [![img](https://mblogthumb-phinf.pstatic.net/MjAxODExMTNfMTUz/MDAxNTQyMDc5NDgxNjk0.BmiifeNUCUCLvhQr_J5CPJnc6bl3QRyIMKqNkMjrXiYg.wMIsOZmbqO7nUZptN6_XY-SXkug8RRTfGxo5iOy8POEg.PNG.greatsk553/image_7528467211542079441488.png?type=w800)](https://m.blog.naver.com/PostView.nhn?blogId=greatsk553&logNo=221397462709&proxyReferer=https:%2F%2Fwww.google.com%2F#)

  [![img](https://mblogthumb-phinf.pstatic.net/MjAxODExMTNfMTEx/MDAxNTQyMDc5NDkxMTk0.9xMOjB3MW9qdG_jYZ5PTJLERJ2NlUmC9XTrrrQvrMLAg.isBC5jhWEhVeuriY-xhHFRlf225pOIbXp8NKjT5cvQQg.PNG.greatsk553/image_1505272081542079454075.png?type=w800)](https://m.blog.naver.com/PostView.nhn?blogId=greatsk553&logNo=221397462709&proxyReferer=https:%2F%2Fwww.google.com%2F#)

  

### URI 스킴의 단점

URI 스킴에 있어서 다음 두 가지 상황을 쉽게 처리할 수 없는 심각한 단점이 존재한다.

1. 앱이 설치되지 않은 경우 : 앱이 설치되어 있지 않은 경우 에러가 발생
2. 두 개 이상의 앱이 myapp://에 응답하려 하는 경우 : 중앙 등록 시스템이 없어 막을 수 있는 방법이 없음

이러한 한계로 **iOS와 안드로이드**는 각각 **유니버설 링크와 앱 링크**로 알려진 2세대 딥 링크 표준을 개발하였다.

## 유니버설 링크와 앱 링크

- **앱 고유 콘텐츠 유형의 URI 스킴**과 달리 유니버설 앱과 앱 링크는 웹 페이지와 앱 내부의 특정 콘텐츠를 가리키는 단순한 **표준 웹 링크**이다. 즉, 일반적으로 **웹사이트 URL과 동일한 문자열**을 사용한다. 

- 유니버설 링크 혹은 앱 링크가 열리면 장치는 **해당 도메인에 등록된 설치된 장치가 있는지 확인**한다. 만약 있다면 웹 페이지를 구동하지 않고 **해당 앱을 즉시 시작**하고, 앱이 없다면 **기본 웹 브라우저**로 해당 웹 URL을(앱 혹은 플레이 스토어로의 간단한 재설정일 수 있음) 시작한다

- 앱에도 특정 URL일때만 동작하도록 설정해줄 필요가 있는데,  Xcode에서는 ‘Associated Domains’ 항목에 연관 도메인을 입력해주는 설정이 필요하다. 이 외에도 요청이 들어온 링크를 앱이 핸들링하는 설정이 추가 되어야 한다.
- Xcode 프로젝트 > Target 메뉴의 대상 프로젝트 > Associated Domains 메뉴에서, applinks: 스킴으로 대상 도메인을 추가하면 된다. (사용자가 앱 설치 시, 여기에 등록된 도메인으로 apple-app-site-association 파일에 대한 요청을 보내게 된다.)
- 유니버셜 링크로 들어오는 요청은, `UIApplicationDelegate 의 application:continueUserActivity:restorationHandler:` 메서드에서, 전달받은 `NSUserActivity` 객체을 사용해 처리할 수 있다.

![universal link](http://www.wisetracker.co.kr/wp-content/uploads/2018/03/universal-link.png)

### 유니버설 링크의 단점

- 애플은 공식적으로 iOS 9.2 버전부터 딥 링크를 위한 URI 링크(URL/URI 스킴)를 더는 지원하지 않으며 개발자는 iOS에서 같은 기능을 위해 유니버설 링크를 구현하도록 강요받았다.
- 하지만 현실적으로는 유니버설 링크는 현재 iOS 표준이지만 실제로는 보편적이지 않다. 유니버설 링크가 작동할 때는 정말 좋지만, 여전히 URI 스킴으로 백업할 필요가 있는 경우가 많다.



## 출처

- [서비스기획을 위한 IT지식 - 딥링크와 스키마](https://onsoo.github.io/%EC%84%9C%EB%B9%84%EC%8A%A4%EA%B8%B0%ED%9A%8D/2018/10/25/deeplink-schema/)
- [유니버설 링크, URI 스킴, 앱 링크 및 딥 링크: 무슨 차이가 있을까요?](https://blog.branch.io/ko/유니버설-링크-uri-스킴-앱-링크-및-딥-링크-무슨-차이가/) 
- [iOS: 유니버셜 링크 적용하기 (앱)](https://ohgyun.com/708)

- [Firebase Dynamic Link를 이용한 Deep Link](https://m.blog.naver.com/PostView.nhn?blogId=greatsk553&logNo=221397462709&proxyReferer=https:%2F%2Fwww.google.com%2F)

- [비개발자를 위한 유니버셜 링크(Universal Link) 핵심 개념](http://www.wisetracker.co.kr/blog/%EB%B9%84%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%A5%BC-%EC%9C%84%ED%95%9C-%EC%9C%A0%EB%8B%88%EB%B2%84%EC%85%9C-%EB%A7%81%ED%81%AC-%ED%95%B5%EC%8B%AC-%EA%B0%9C%EB%85%90/)

