 # Cookie (feat. HTTPCookieStorage, WKHTTPCookieStore)

## Cookie란?

- 웹 사이트로부터 받아 **사용자의 브라우저에 저장하는 작은 데이터**
- 웹 사이트를 로드할 때 마다, **사용자의 활동과 식별을 위해 서버로 다시 돌려보내**는데 활용
- 상태 정보를 기억하고, 버튼 클릭, 브라우징 하스토리, 로그인 등 브라우저의 활동을 기록하는데 필요해서 만들어짐
- 기본적으로 자신을 보낸 서버의 도메인을 가지고 있고, 해당 도메인을 연결할때마다 사용됨

## iOS 에서 쿠키 사용하기

- iOS에서 **쿠키 관리는 자동**으로 이뤄짐
- Cookie의 속성에는 domain, path, name, value 값이 있는데, 해당 domain으로 이동할때마다 저장된 쿠키가 사용됨 
- UIWebView냐, WKWebView 냐에 따라 사용하는 쿠키 저장소가 달라짐

### UIWebView (deprecated)

- 쿠키 저장소 : **HTTPCookieStorage** (앱의 shared cookie storage를 상속 받음).  `HTTPCookieStorage.shared`로 접근 가능.
- 쿠키 저장 여부 : 휘발성 (앱이 메모리에서 해제되면 쿠키도 삭제됨)
- 프로토콜 상에서 지원하는 '**Set-Cookie**'라는 키를 통해 서버에서 보내준다면 iOS에서는 **HTTPCookieStorage에 자동 저장**, 자동 사용 됨
- 만약 **개발자가 임의로 쿠키를 설정**해야한다면 **'Cookie'를 키로 URLRequest 헤더에 추가**해야함. 그러면 HTTPCookieStorage에 저장된 값 자동 무시 됨

### **WKWebView**

- 쿠키 저장소 : **WKHTTPCookieStore** (자신만의 쿠키 저장소를 가짐). `WKWebsiteDataStore.default().httpCookieStore` 로 접근 가능.
- 쿠키 저장 여부 : 비휘발성 (앱이 메모리에서 해제돼도 유지됨)

- 여러 **웹 뷰에 쿠키를 공유**하기 위해선 **같은 WKProcessPool 객체를 사용**하면 됨

# 출처

- [[iOS\] iOS에서 쿠키 사용하기 (NSHTTPCookieStorage, NSHTTPCookie](https://maskkwon.tistory.com/193)
- [HTTPCookieStorage](https://developer.apple.com/documentation/foundation/httpcookiestorage)
- [UIWebView WKWebView 비교 및 쿠키 저장 방법](https://zetal.tistory.com/entry/UIWebView-WKWebView-%EB%B9%84%EA%B5%90-%EB%B0%8F-%EC%BF%A0%ED%82%A4-%EC%A0%80%EC%9E%A5-%EB%B0%A9%EB%B2%95)
