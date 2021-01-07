# Hystrix

- Netflix에서 [Circuit Breaker Pattern](https://martinfowler.com/bliki/CircuitBreaker.html)을 구현한 라이브러리
- Micro Service Architecture에서 장애 전파 방지를 할 수 있다. 
- 즉 분산된 서비스간 통신이 원활하지 않은 경우에 각 서비스가 장애 내성과 지연 내성을 갖게하도록 도와주는 것으로 키워드는 통신 문제 극복.
- Thread Timeout, 장애 대응 등을 설정해 장애시 정해진 루트를 따르도록 할 수 있다



출처:  [기본기를 쌓는 정아마추어 코딩블로그]


## 참고

- [(Spring Cloud) Hystrix](https://supawer0728.github.io/2018/03/11/Spring-Cloud-Hystrix/)
- [Springboot hystrix 사용기 (hystrix로 마이크로 서비스 간의 서비스 호출 실패를 방지해보자](https://jeong-pro.tistory.com/183#recentComments)
