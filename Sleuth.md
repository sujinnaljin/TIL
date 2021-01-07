# Sleuth

- Spring에서 공식적으로 지원하는 **Zipkin Client Library**
- **MSA환경**에서 **클라이언트의 호출**은 내부적으로 여러 마이크로서비스를 거쳐서 일어나기 때문에 추적이 어렵다. 때문에 이를 **추적**하기 위해서는 **연관된 ID**가 필요한데, 이런 ID를 자동으로 **생성**해주는 것이 **Spring Cloud Sleuth**이다.
- 호출되는 서비스에 Trace(추적) ID와 Span(구간) ID를 부여한다. Trace ID는 클라이언트 호출의 시작부터 끝날때까지 동일한 ID로 처리되며, Span ID는 마이크로서비스당 1개의 ID가 부여된다. 이 두 ID를 활용하면 클라이언트 호출을 쉽게 추적할 수 있다.



![img](https://blog.kakaocdn.net/dn/qLYDg/btqFhljc86O/KWUSGninlxOTcAxALKToj1/img.png)



## 출처

-  [Zipkin과 Sleuth를 활용한 분산 환경 로그 트레이싱](https://twofootdog.tistory.com/65)

