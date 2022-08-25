# HTTP header

- 헤더를 통해 HTTP 전송에 필요한 모든 부가정보 관리

## 목차
- [표현 관련 헤더](#표현-관련-헤더)
- [협상(콘텐츠 네고시에이션) 관련 헤더](#협상콘텐츠-네고시에이션-관련-헤더)
- [전송 관련 헤더](#전송-관련-헤더)
- [일반 정보 관련 헤더](#일반-정보-관련-헤더)
- [특별한 정보 관련 헤더](#특별한-정보-관련-헤더)
- [인증 관련 헤더](#인증-관련-헤더)
- [쿠키 관련 헤더](#쿠키-관련-헤더)
- [캐시 관련 헤더](#캐시-관련-헤더)

## 표현 관련 헤더

- **표현**은 요청이나 응답에서 전달할 **실제 데이터**

- **표현 헤더**는 **표현 데이터**를 **해석**할 수 있는 정보 제공
- 표현 헤더는 **전송, 응답** 둘다 사용

### 종류

- `Content-Type` : 표현 **데이터의 형식** 
  - text/html; charset=utf-8
  - application/json
  - image/png
- `Content-Encoding` : 표현 데이터의 **압축 방식**. 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
  - gzip 
  - deflate 
  - identity
- `Content-Language` : 표현 데이터의 **자연 언어** 
  - ko
  - en
  - en-US
- `Content-Length` : 표현 데이터의 **길이**. byte 단위. Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안됨

## 협상(콘텐츠 네고시에이션) 관련 헤더

- **협상**은 **클라이언트가 선호하는 표현** 요청. 최대한 이 형식으로 줄 수 있도록 서버야 노력해줘라..

- 협상 헤더는 **요청시**에만 사용

- 선호하는 표현의 우선 순위를 **Quality Values(q)** 값 이용

  0~1, **클수록 높은 우선순위**. 생략하면 1

  ```
  Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
  ```

  1. ko-KR;q=1 (q생략)

  2. ko;q=0.9

  3. en-US;q=0.8 

  4. en:q=0.7

- 선호하는 표현이 구체적일수록 우선함

  ```
  Accept: text/*, text/plain, text/plain;format=flowed, */*
  ```

  1. text/plain;format=flowed

  2. text/plain
  3. text/*

  4. **/* *

- 구체적인 것을 기준으로 미디어 타입을 맞춤. 

  ```
  Accept: text/*;q=0.3, text/html;q=0.7, text/html;level=1, text/html;level=2;q=0.4, */*;q=0.5
  ```

  | 미디어 타입       | 퀄리티 |
  | ----------------- | ------ |
  | text/html;level=1 | 1      |
  | text/html         | 0.7    |
  | text/plain        | 0.3    |
  | image/jpeg        | 0.5    |
  | text/html;level=2 | 0.4    |
  | text/html;level=3 | 0.7    |

- 구글에 hello 검색 쳤을때 협상 관련 요청 헤더

  ![image](https://user-images.githubusercontent.com/20410193/186596388-0b9fedcb-dcd8-4a4c-b3f4-bd5ef87202ce.png)


### 종류

- `Accept` : 클라이언트가 선호하는 **미디어 타입** 전달 (Content-Type)
  - text/html; charset=utf-8
  - application/json
  - image/png
- `Accept-Charset` : 클라이언트가 선호하는 **문자 인코딩** 
  - utf-8
  - iso-8859-1
- `Accept-Encoding` : 클라이언트가 선호하는 **압축 인코딩** 
  - gzip 
  - deflate 
  - identity
- `Accept-Language` : 클라이언트가 선호하는 **자연 언어**
  - ko
  - en
  - en-US

## 전송 관련 헤더

### 종류

- 단순 전송 - `Content-Length` 만 들어감

  ```
  Content-Length: 3423
  ```

- 압축 전송 - `Content-Encoding` 이 추가로 들어감

  ```
  Content-Encoding: gzip 
  Content-Length: 521
  ```

- 분할 전송 - `Transfer-Encoding`. 이때는 `Content-Length` 넣으면 안됨.

  ```
  Transfer-Encoding: chunked
  ```

- 범위 전송 - 클라이언트에서 `Range` 헤더에 범위 지정해서 요청. 서버에서는  `Content-Range` 에 범위와 끝 길이 응답.

  ```
  // 요청
  Range: bytes=1001-2000
  
  // 응답
  Content-Range: bytes 1001-2000 / 2000
  ```

## 일반 정보 관련 헤더 

### 종류 

- `From` : 유저 에이전트의 **이메일 정보**. 일반적으로 잘 사용되지 않음. 검색 엔진 같은 곳에서 주로 사용. 요청에서 사용

- `Referer` : **이전 웹 페이지 주소**. A -> B로 이동하는 경우 B를 요청할 때 `Referer: A` 를 포함해서 요청.

  Referer를 사용해서 **유입 경로 분석** 가능. 요청에서 사용. 

  참고로 referer는 단어 referrer의 오타

- `User-Agent` : 유저 에이전트 **애플리케이션 정보** (웹 브라우저 정보 등). 

  어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능. 통계 정보 파악 가능. 요청에서 사용.

  ```
  user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/ 537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36
  ```

- `Server` : **요청을 처리**하는 오리진 **서버**의 소프트웨어 **정보**. 응답에서 사용

  ```
  Server: Apache/2.2.22 (Debian)
  server: nginx
  ```

- `Date` : 메시지가 생성된 **날짜와 시간**. 응답에서 사용

  ```
  Date: Tue, 15 Nov 1994 08:12:31 GMT
  ```

## 특별한 정보 관련 헤더

### 종류

- `Host` : 요청한 **호스트 정보** (도메인). 요청에서 사용. **필수** 헤더. 

  하나의 서버가 여러 도메인을 처리해야 할 때, 즉 하나의 IP 주소에 여러 도메인이 적용되어 있을 때 등의 문제를 해결하기 위해 필수임. 

- `Location` : 페이지 **리다이렉션**. 웹 브라우저는 **3xx 응답의 결과**에 **Location 헤더**가 있으면, Location 위치로 **자동 이동** (리다이렉트)

- `Allow` : 허용 가능한 HTTP 메서드. 405 (Method Not Allowed) 에서 응답에 포함해야함. 그런데 그렇게 많이 구현되어있진 않음.

  ```
  Allow: GET, HEAD, PUT // 이 값 보고 아~ post 는 불가능한 요청이군 알 수 있음
  ```

- `Retry-After` : 유저 에이전트가 **다음 요청**을 하기까지 **기다려야 하는 시간**

  503 (Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음

  ```
  Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기) 
  Retry-After: 120 (초단위 표기)
  ```

## 인증 관련 헤더

### 종류

- `Authorization` : 클라이언트 **인증 정보**를 서버에 전달 

  ```
  Authorization: Basic xxxxxxxxxxxxxxxx
  ```

- `WWW-Authenticate` : 리소스 접근시 필요한 **인증 방법** 정의. 401 Unauthorized 응답과 함께 사용.

  ```
  WWW-Authenticate: Newauth realm="apps", type=1, title="Login to \"apps\"", Basic realm="simple"
  ```

## 쿠키 관련 헤더

### 종류

- `Set-Cookie` : **서버에서 클라이언트**로 **쿠키 전달** (응답)

  사용자 **로그인 세션 관리**, **광고** 정보 트래킹에 사용

  세팅된 쿠키 정보는 **항상 서버에 전송**됨. 네트워크 **추가 트래픽** 유발하기 때문에 **최소한의 정보**만 사용(세션 id, 인증 토큰). 

  서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 (localStorage, sessionStorage) 참고

  보안에 민감한 데이터는 저장하면 안됨(주민번호, 신용카드 번호 등등)

  ```
  set-cookie: sessionId=abcde1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/; domain=.google.com; Secure
  ```

- `Cookie` : **클라이언트**가 서버에서 받은 **쿠키를 저장**하고, HTTP **요청시** **서버**로 **전달**

#### 쿠키 - 생명주기

- `Expires` : 만료일이 되면 쿠키 삭제

  ```
  expires=Sat, 26-Dec-2020 04:39:21 GMT
  ```

- `max-age` : 0이나 음수를 지정하면 쿠키 삭제

  ```
  max-age=3600 (3600초)
  ```


- **세션** 쿠키: **만료 날짜**를 **생략**하면 **브라우저 종료**시 까지만 유지 
- **영속** 쿠키: **만료 날짜**를 **입력**하면 **해당 날짜**까지 유지

#### 쿠키 - 도메인

- `domain`

  ```
  domain=example.org
  ```

  - domain 명시 - 명시 도메인 + **서브 도메인 포함**

    `domain=example.org` 지정해서 쿠키 생성 => example.org는 물론이고 dev.example.org도 쿠키 접근 

  - domain 생략  - 현재 도메인만 적용

    `example.org` 에서 쿠키를 생성하고 domain 지정을 생략 => example.org 에서만 쿠키 접근

#### 쿠키 - 경로

- `path` - 이 경로를 포함한 하위 경로 페이지만 쿠키 접근. 일반적으로 `path=/` 루트로 지정

  ```
  path=/home
  ```

  - /home -> 가능 
  - /home/level1 -> 가능 
  - /home/level1/level2 -> 가능
  - /hello -> 불가능

#### 쿠키 - 보안

- `Secure` :  쿠키는 원래 **http, https를 구분하지 않**고 전송하는데, **Secure를 적용**하면 **https인 경우에만** 전송.

- `HttpOnly` : XSS 공격 방지.  자바스크립트에서 접근 불가(document.cookie). HTTP 전송에만 사용

- `SameSite` :  XSRF 공격 방지. 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송. 기능 적용된지 몇년 안돼서 브라우저에서 어느정도 지원하는지까지 확인하고 사용해야함.

  

## 캐시 관련 헤더

### 캐시 제어 헤더

- `cache-control` : 캐시 제어. 웹브라우저 **자체 내에 캐시**가 있어서 **응답 저장** 가능.

  캐시 지시어 (directives) 로는 아래와 같은 종류들이 있음

  - `max-age` : **캐시**가 **유효한 시간**. 초 단위

    ```
    cache-control: max-age=60
    ```

  - `no-cache` : 데이터는 **캐시해도 되지**만, **항상 원(origin) 서버에 검증**하고 사용 (modified 되지 않았니..? 등등)

    ```
    cache-control: no-cache
    ```

  - `no-store` : 데이터에 민감한 정보가 있으므로 저장하면 안됨  (메모리에서 사용하고 최대한 빨리 삭제)

  - `must-revalidate` : 캐시 만료후 최초 조회시 **원 서버에 무조건 검증** 필요.

    언뜻보면 `no-cache` 만 사용해도 항상 원 서버에서 검증하니까 이 지시어는 필요 없는거 아니야? 생각할 수 있음.

    하지만 **`no-cache`** 로 설정된 경우에는 네트워크 등의 이슈로 **프록시 캐시 서버에서 원 서버에 접근 불가**한 경우, 캐시 서버 설정에 따라서 "그래.. 오류 보다는 오래된 데이타라도 보여주자" 하고 **프록시 서버의 캐시 데이터를 반환**할 수 있음.

    하지만 `must-revalidate` 지시어는 **원 서버에 접근할 수 없**는 경우, `504 Gateway Timeout` 과 함께 **항상 오류**가 발생해야 함 

    ```
    Cache-Control: must-revalidate
    ```

- `Pragma`  : 캐시 제어. HTTP 1.0 하위 호환. 지금은 거의 사용하지 않음.

  ```
  Pragma: no-cache
  ```

- `Expires` : 캐시 만료일을 **정확한 날짜**로 지정. 하위 호환. HTTP 1.0 부터 사용

  지금은 **더 유연**한 **`Cache-Control: max-age`** 권장. Cache-Control: max-age와 함께 사용하면 `Expires`는 무시.

  ```
  expires: Mon, 01 Jan 1990 00:00:00 GMT
  ```

### 검증 헤더와 조건부 요청 헤더

- 검증 헤더 : **캐시 데이터**와 **서버 데이터**가 **같은지** 검증하는 데이터
  - `Last-Modified`
  - `ETag` : Entity Tag

- 조건부 요청 헤더 : 검증 헤더로 조건에 따른 분기.  조건이 만족하면 200 OK, 조건이 만족하지 않으면 304 Not Modified.
  - `If-Modified-Since` : Last-Modified 와 짝지어 사용. 반대는 `If-Unmodified-Since` 헤더.
  - `If-None-Match` : ETag 와 짝지어 사용. 반대는 `If-Match` 헤더.

#### Last-Modified 와 If-Modified-Since

캐시 **유효 시간이 초과**해서 서버에 **다시 요청**하면,  기존 데이터가 변경되었을 수도 있지만 **변경되지 않았을 수도** 있음. -> 그럼 브라우저 자체 **캐시**에 있는 데이터 그대로 **사용**하면 되잖아? 

단 **클라이언트**의 데이터와 **서버**의 **데이터가 같다**는 사실을 **확인**할 수 있는 방법 필요 -> `Last-Modified` & `if-modified-since`

- `Last-Modified` :  데이터가 마지막으로 수정된 시간. 응답에서 사용.

  ```
  Last-Modified: 2020년 11월 10일 10:00:00
  ```

- `if-modified-since` : 캐시가 가지고 있는 데이터 최종 수정일. 요청에서 사용

  ```
  if-modified-since: 2020년 11월 10일 10:00:00
  ```

**Flow**

1. 응답에서 `Last-Modified` 내려주면 그 값 갖고 있다가, 캐시 만료시간 되어서 재요청할때 `if-modified-since` 헤더에 담아보냄. 

2. 서버에서 `if-modified-since` 헤더 값 보고, 그 이후로 데이터 수정된게 없으면 **`304 Not Modified` 상태코드와 함께 헤더만** 내림. body 는 생략해서 크기 작아짐.

   ```
   HTTP/1.1 304 Not Modified
   Content-Type: image/jpeg
   cache-control: max-age=60
   Last-Modified: 2020년 11월 10일 10:00:00
   Content-Length: 34012
   ```

3. 클라이언트는 304 응답 보고 기존 **캐시 데이터 재사용** 및 **헤더 데이터 갱신** (캐시 유효 시간 등) . 결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 **헤더 정보만 다운**로드.

**단점**

- **1초 미만(0.x초) 단위**로 **캐시 조정이 불가**능. `2020년 11월 10일 10:00:00` 이런식으로 초단위로 끝나기 때문에
- 날짜 기반이라는 한정된 로직 사용
- 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우 (ex. A -> B -> A 수정하면 결국 A 로 수정된 것) 에도 전체 다운로드
- 서버에서 별도의 캐시 로직을 관리하고 싶은 경우 (ex. 스페이스나 주석처럼 크게 영향이 없는 변경에서 캐시를 유지하고 싶은 경우) 컨트롤 불가

#### ETag 와 If-None-Match

위에서 설명한 Last-Modified 와 If-Modified-Since 의 단점을 보완할 수 있는 방법 

- `Etag` :  Entity Tag. 캐시용 데이터에 **임의의 고유한 버전** 이름을 달아둠. 그리고 **데이터가 변경**되면 이 이름을 바꾸어서 **변경**함 (**Hash를 다시** 생성)

  ```
  ETag: "v1.0"
  ETag: "a2jiodwjekjl3"
  ```

- `If-None-Match` : 캐시가 가지고 있는 `ETag` 값.

  ```
  If-None-Match: "a2jiodwjekjl3"
  ```

**Flow**

1. 응답에서 `ETag` 내려주면 그 값 갖고 있다가, 캐시 만료시간 되어서 재요청할때 `If-None-Match` 헤더에 담아보냄. 
2. 서버에서 `If-None-Match` 헤더 값 보고, 이 값이 자기가 갖고 있는 `ETag` 와 같으면  **`304 Not Modified` 상태코드와 함께 헤더만** 내림. body 는 생략해서 크기 작아짐.
3. 클라이언트는 304 응답 보고 기존 **캐시 데이터 재사용** 및 **헤더 데이터 갱신** (캐시 유효 시간 등) . 결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 **헤더 정보만 다운**로드.

**장점**

-  **캐시 제어 로직을 서버에서 완전히 관리**
-  클라이언트는 단순히 이 값을 서버에 제공(클라이언트는 캐시 메커니즘을 모름). 그냥 단순하게 ETag만 서버에 보내서 같으면 유지, 다르면 다시 받음

### 프록시 캐시

응답 속도를 빠르게 하기 위해 멀리 있는 원 서버와 클라이언트 사이에 보통 프록시 캐시 서버를 둠.

**프록시 캐시 서버**에 저장되는 캐시를 **public 캐시**, **클라이언트 로컬**에 저장되는 캐시를 **private 캐시**라고 함.

#### 종류

- **`Cache-Control` 의 캐시 지시어** (directives) 를 사용

  - `public ` : 응답이 public 캐시에 저장되어도 됨 

    ```
    Cache-Control: public
    ```

  - `private` : 응답이 해당 사용자만을 위한 것임, private 캐시에 저장해야 함(기본값) 

    ```
    Cache-Control: private
    ```

  - `s-maxage` : 프록시 캐시에만 적용되는 max-age

    ```
    Cache-Control: s-maxage
    ```

- `Age` : 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)

  ```
  Age: 60
  ```

### 캐시 무효화

- **`Cache-Control` 의 캐시 지시어** (directives) 와 **`Pragma` 를 함께 사용**해서 **확실한 캐시 무효화** 가능. 

  캐시 적용 안해도 get 요청인 경우에 임의로 캐시해버리는 경우도 있기 때문에, 이 페이지는 찐으로 캐시되면 안돼!! 하면 아래 값들 모두 입력

  ```
  Cache-Control: no-cache, no-store, must-revalidate
  Pragma: no-cache // 혹시 모를 http 1.0 대비
  ```

# 출처

- [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)
