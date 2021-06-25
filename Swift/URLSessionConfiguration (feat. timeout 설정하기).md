# URLSessionConfiguration (feat. timeout 설정하기)

- 기본적으로 네트워크 요청은 **shared singleton session** 을 이용해서 보냄

  - shared session의 특징은 아래와 같음
    - configure 불가
    - session과 interact를 위해 delegate 설정 불가
    - background network 요청 수행을 위해 사용 불가

  ```swift
  let session = URLSession.shared
  let task = session.dataTask(with: url) { data, response, error in
    // handle result in completion handler
  }
  task.resume()
  ```

- shared 객체를 사용하지 않고 `URLSessionConfiguration` 객체를 인자로 받는 URLSession을 만들 수도 있음 (주입하고 난 후에 세션을 바꾸는 것은 영향 없음)

  ```swift
  let configuration = URLSessionConfiguration.default
  // change the default configuration
  // before creating the session
  let session = URLSession(configuration: configuration)
  ```

- URLSessionConfiguration 옵션에는 아래와 같은 것들이 존재

  - 기본 request / response 타임아웃 설정

    ```swift
    configuration.timeoutIntervalForRequest = 30 //default는 60초
    ```

  - 연결이 설정될 때까지 대기

    ```swift
    configuration.waitsForConnectivity = true
    ```

  - 저데이터 모드에서 네트워크를 사용하는 것이나, 셀룰러 데이터 사용하는 것 방지

    ```swift
    configuration.allowsConstrainedNetworkAccess = false
    configuration.allowsCellularAccess = false 
    ```

  - shared URL cache를 이용하는 대신 커스텀하게 설정 가능 (기본 캐시 memoryCapacity = 512K, diskCapacity = 10Mb)

    ```swift
    let cache = URLCache(memoryCapacity: 500_000_000, 
                           diskCapacity: 1_000_000_000)
    configuration.urlCache = cache
    ```

  - 모든 요청에 HTTP 헤더를 추가

    ```swift
    configuration.httpAdditionalHeaders = ["User-Agent": "MyApp 1.0"]
    ```


# 출처

- [URLSessionConfiguration Quick Guide](https://useyourloaf.com/blog/urlsessionconfiguration-quick-guide/)
