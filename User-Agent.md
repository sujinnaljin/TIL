# User-Agent

- HTTP 요청을 보내는 디바이스와 브라우저 등 **사용자 소프트웨어의 식별 정보를 담고 있는 request header**의 한 종류
- 임의로 수정될 수 없는 값
- HTTP 요청 에러가 발생했을 때 요청을 보낸 **사용자 환경**을 알아보기 위해 사용

- 보통 **Mozilla 정보/버전 + 운영체제 정보 + 렌더링 엔진 정보 + 브라우저** 형태로 노출되는 듯

  ```
  Mozilla/5.0 (platform; rv:geckoversion) Gecko/geckotrail Firefox/firefoxversion
  ```

  - Mozilla/5.0 : 접속한 브라우저가 Mozilla와 호환된다는 의미. 거의 모든 브라우저가 이렇게 표시 됨

  - platform : 브라우저가 실행되는 운영체제 환경(window, mac, linux, android 등), 그리고 모바일인지 여부.

    rv: geckoversion : Gecko 버전 (파이어폭스의 렌더링 엔진)

  - Gecko/geckotrail : 브라우저가 Gecko 기반인지 여부.
  - Firefox/firefoxversion : 브라우저가 파이어폭스라는 의미, 그리고 파이어폭스의 버전.

- Safari 예시 - 마지막 브라우저 정보에 Safari가 노출되고, 모바일 접속일 경우 Mobile이라고 같이 뜬다.

  ```
  Mozilla/5.0 (iPhone; CPU iPhone OS 13_5_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.1.1 Mobile/15E148 Safari/604.1
  ```

- 이걸 iOS 에서 custom으로 설정해주려면 아래처럼 `"UserAgent"` 라는 키값으로 register 해주면 됨

  헤더에서 실제로 나가는 key 값은 `User-Agent` 면서 왜 코드로 세팅할땐 hypen 없는 `UserAgent` 냐? 영감탱 지대 짱남..

  ```swift
  UserDefaults.standard.register(defaults: ["UserAgent": newUserAgent])
  ```

# 출처

- [User-agent 정확하게 해석하기](https://velog.io/@ggong/User-agent-%EC%A0%95%ED%99%95%ED%95%98%EA%B2%8C-%ED%95%B4%EC%84%9D%ED%95%98%EA%B8%B0)

-  [How can I set the "User-Agent" header of a UIWebView in Swift](https://stackoverflow.com/questions/26219997/how-can-i-set-the-user-agent-header-of-a-uiwebview-in-swift)

