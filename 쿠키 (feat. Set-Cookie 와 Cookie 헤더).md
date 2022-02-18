# 쿠키 (feat. Set-Cookie 와 Cookie 헤더)

- 쿠키는 **브라우저에 저장되는 작은 크기의 문자열**로, [RFC 6265](https://tools.ietf.org/html/rfc6265) 명세에서 정의한 **HTTP 프로토콜의 일부**

- 쿠키는 주로 **웹 서버에 의해 생성**됨. HTTP 요청을 수신할 때, 서버는 응답과 함께 **[`Set-Cookie`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Set-Cookie) 헤더를 전송**할 수 있음

  ```
  HTTP/1.0 200 OK
  Content-type: text/html
  Set-Cookie: yummy_cookie=choco
  Set-Cookie: tasty_cookie=strawberry
  
  [page content]
  ```

- 서버가 HTTP 응답 헤더(header)의 **`Set-Cookie` 에 내용을 넣어 전달**하면, 브라우저는 이 내용을 자체적으로 **브라우저에 저장**함. 이게 바로 쿠키!

- 그 후 **쿠키**는 같은 서버에 의해 만들어진 **요청(Request)들의 [`Cookie`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Cookie) HTTP 헤더안에 포함**되어 **전송** . 즉, 서버로 전송되는 모든 요청과 함께, 브라우저는 **[`Cookie`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Cookie) 헤더**를 사용하여 서버로 이전에 저장했던 **모든 쿠키들을 회신**

  ```
  GET /sample_page.html HTTP/1.1
  Host: www.example.org
  Cookie: yummy_cookie=choco; tasty_cookie=strawberry
  ```

- 쿠키는 클라이언트 식별과 같은 **인증**에 가장 많이 쓰임.

  1. 사용자가 **로그인**하면 **서버**는 **HTTP 응답 헤더의 `Set-Cookie`** 에 담긴 “**세션 식별자**(session identifier)” 정보를 사용해 **쿠키를 설정**
  2. 사용자가 **동일 도메인에 접속**하려고 하면 **브라우저**는 HTTP **`Cookie` 헤더**에 인증 정보가 담긴 고유값(**세션 식별자**)을 함께 실어 **서버에 요청**을 보냄
  3. 서버는 브라우저가 보낸 요청 **헤더**의 세션 식별자를 읽어 **사용자를 식별**

- `document.cookie` 프로퍼티를 이용하면 브라우저에서도 쿠키에 접근 가능

  ```javascript
  //지금 보고 있는 이 사이트와 관련된 쿠키가 브라우저에 저장되어있는지 알아보기
  console.log( document.cookie ); // cookie1=value1; cookie2=value2;...
  ```

  `document.cookie`는 **`name=value` 쌍**으로 구성되어있고, 각 쌍은 **`;`로 구분**.

  ![image](https://user-images.githubusercontent.com/20410193/154632131-860101e4-b628-475f-8c95-c3603d7f7885.png)

  

- 쿠키의 **한계**

  - `encodeURIComponent`로 인코딩한 이후의 `name=value` 쌍은 4KB를 넘을 수 없음. 이 용량을 넘는 정보는 쿠키에 저장할 수 없음.

  - 도메인 하나당 저장할 수 있는 쿠키의 개수는 20여 개 정도로 한정. 개수는 브라우저에 따라 조금씩 다름.

- 쿠키의 **라이프타임**은 두가지 방법으로 정의 가능

  - **세션 쿠키**는 **현재 세션이 끝날 때 삭제**됨. 브라우저는 "현재 세션"이 끝나는 시점을 정의하며, 어떤 브라우저들은 재시작할 때 세션을 복원해 세션 쿠키가 무기한 존재할 수 있도록 함.

  - 영속적인 쿠키는 `Expires` 속성에 명시된 날짜에 삭제되거나, `Max-Age` 속성에 명시된 기간 이후에 삭제

    ```javascript
    Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
    ```

# 출처

- [HTTP 쿠키](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies)
- [쿠키와 document.cookie](https://ko.javascript.info/cookie)
