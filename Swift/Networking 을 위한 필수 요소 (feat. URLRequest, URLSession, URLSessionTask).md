# Networking 을 위한 필수 요소 (feat. URLRequest, URLSession, URLSessionTask)

## struct **`URLRequest`**

- `URLRequest` 는 load request 의 필수 속성인 **URL**과 로드하는 데 사용되는 **정책**을 캡슐화
- `URLRequest` 가 **HTTP 및 HTTPS 요청**일 경우 HTTP **method**( `GET`, `POST`등) 및 HTTP **header**가 포함
- `URLRequest` 는 **request에 대한 정보**만 나타내고, [`URLSession`](https://developer.apple.com/documentation/foundation/urlsession) 과 같은 다른 클래스를 사용해서 서버에 요청을 보냄.

- 아래와 같은 특정 헤더 필드는 예약되어 있음 (참고: [Reserved HTTP Headers](https://developer.apple.com/documentation/foundation/nsurlrequest#1776617))

  ```
  Content-Length
  Authorization
  Connection
  Host
  Proxy-Authenticate
  Proxy-Authorization
  WWW-Authenticate
  ```

## class **`URLSession`**

- 관련된 **네트워크 데이터 전송 작업 그룹을 조정**하는 object

- 앱은 관련 **데이터 전송 작업 그룹을 조정**하는 **하나 이상의 URLSession 인스턴스**를 만듦. 예를 들어 웹 브라우저를 만드는 경우 앱은 tab이나 window당 세션을 하나씩 만들거나, interactive use 세션을 만들고 백그라운드 다운로드를 위한 세션을 따로 만들 수 있음. 각 세션 내에서 앱은 특정 URL에 대한 요청을 나타내는 일련의 작업을 추가 (필요한 경우 HTTP 리디렉션 수행).

- **configuration과 caching 을 공유**하는 **개별 request 를 만들**기 위해 사용 됨

- **URLSession.shared** 는 URLSession에서 기본 제공하는 singleton 객체로 **별도의 URLSession 생성 없이 URLSession을 사용**할 수 있음

  - 장점 - Configuration을 활용하는 것보다 상대적으로 가볍게 작업 처리가 가능. (**속도**가 좀 더 빠르다고 함.)
  - 단점 - delagate와 configuration을 사용할 수 없음

- URLSession.shared 를 사용하는 대신, **URLSessionConfiguration**을 통해 다음 세가지 유형의 URLSession 생성 가능

  1. default Session : shared 세션과 비슷하게 동작하지만 configure 가능. 기본 세션에 **delegate를 할당**하여 데이터를 점진적으로 가져올 수도 있음. 또한 **데이터를 디스크에 저장**
  2. ephemeral Session: shared 세션과 유사하지만 **캐시, 쿠키 또는 자격 증명을 디스크에 기록하지 않**고, **메모리에 올려 세션과 연결**. 따라서 애플리케이션이 **세션을 만료시키면 세션과 관련한 데이터가 사라짐**.
  3. **백그라운드** 세션 (Background Session) : 앱이 실행되지 않는 동안 **백그라운드에서 콘텐츠 업로드 및 다운로드**를 수행 가능

  ```swift
  let defaultSession = URLSession(configuration: .default)
  let ephemeralSession = URLSession(configuration: .ephemeral)
  let backgroundSession = URLSession(configuration: .background)
  ```

- alamofire , moya와 같은 네트워크 라이브러리도 **URLSession을 기반**으로 함

- [`URLSession`](https://developer.apple.com/documentation/foundation/urlsession) 은 **데이터 전송을 위한 API**를 제공. 즉 URLSession 객체를 생성하고, API 를 통해 원하는 task를 붙여서 원하는 네트워크 통신 작업을 수행하는 것. 

  - [`dataTask(with:)`](https://developer.apple.com/documentation/foundation/urlsession/1411554-datatask) ->  [`URLSessionDataTask`](https://developer.apple.com/documentation/foundation/urlsessiondatatask)
  - [`uploadTask(with:from:)`](https://developer.apple.com/documentation/foundation/urlsession/1409763-uploadtask)  -> [`URLSessionUploadTask`](https://developer.apple.com/documentation/foundation/urlsessionuploadtask) 
  - [`downloadTask(with:)`](https://developer.apple.com/documentation/foundation/urlsession/1411482-downloadtask) -> [`URLSessionDownloadTask`](https://developer.apple.com/documentation/foundation/urlsessiondownloadtask)

## class **`URLSessionTask`**

- 네트워크 요청을 위한 **data transfer** 을 수행하는 작업 객체(task object)를 나타내는 추상 클래스
- URLSession에 있는 task의 기본 클래스. 해당 class 를 직접적으로 사용하진 않지만, 아래와 같은 subclass 들을 사용하는 함으로써 **데이터를 가져오거나 파일을 업로드, 다운로드 하는 작업을 수행**
  - `URLSessionDataTask` - 서버에서 **데이터를 받아오**는 task를 수행
  - `URLsessionUploadTask` - 웹서버로 **파일을 업로드**할때 이 task를 수행
  - `URLSessionDownloadTask` - 서버로부터 **데이터를 다운로드** 받아서 파일의 형태로 저장하는 task를 수행
- **Task는 항상 세션의 일부**이므로 **URLSession 인스턴스에서 태스크 생성 방법 중 하나를 호출하여 태스크를 생성**. 호출하는 메서드에 따라 task type 이 결정됨

## 사용 예시

```swift
func requestGet(url: String, completionHandler: @escaping (Bool, Any) -> Void) {
    guard let url = URL(string: url) else {
        print("Error: cannot create URL")
        return
    }
    
    var request = URLRequest(url: url)
    request.httpMethod = "GET"
    
    URLSession.shared.dataTask(with: request) { data, response, error in
        //처리
    }.resume() //모든 task()는 일시정지 상태로 시작하기 때문에 resume()으로 task()를 시작해야
}
```



# 출처

- [Simplest Networking Layer in iOS](https://medium.com/swift2go/simplest-networking-layer-in-ios-58193fe562c8)
- [iOS - 네트워크 통신 (URLSession)](https://jinsangjin.tistory.com/98)
- [URLRequest](https://developer.apple.com/documentation/foundation/urlrequest)
- [NSURLRequest](https://developer.apple.com/documentation/foundation/nsurlrequest#1776617)
- [URLSession](https://developer.apple.com/documentation/foundation/urlsession)
- [URLSessionTask](https://developer.apple.com/documentation/foundation/urlsessiontask)

