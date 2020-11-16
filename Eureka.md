# Eureka (유레카)

- AWS와 같은 **Cloud 시스템**에서 서비스의 **로드 밸런싱과 실패처리 등을 유연하게 가져가 위해 각 서비스들의 IP / Port / InstanceId를 가지고 있는 REST 기반의 미들웨어 서버**
- 마이크로 서비스 기반의 아키텍처의 핵심 원칙 중 하나인 **Service Discovery** 의 역할을 수행
- **MSA에서는 Service의 IP와 Port가 일정하지 않고 지속적을 변화** 하기 때문에 Client에 Service의 정보를 **수동으로 입력하는 것은 한계** 존재
- 따라서 가용한 서비스 인스턴스 목록과 그 **위치(host, port)가 동적으로 변하는 가상화 혹은 컨테이너화된 환경**에서 **클라이언트가 서비스 인스턴스를 호출**할 수 있도록 Service registry를 제공/관리

### 아키텍처 및 플로우



![img](https://blog.kakaocdn.net/dn/cRtMzj/btqBLjccv9N/7bPaaBbFoPjf7zTKYl5v1K/img.png)

- eureka에 각자의 ip / port를 등록하여 http 통신에 사용
- Eureka는 Client-Server의 방식으로 **Eureka Server는 모든 Client 서버들이 본인의 IP와 Port, InstanceId를 Eureka-Server로 전달**합니다. 그리고 **Eureka에 있는 정보를 Fetch하여 Eureka-Client간 통신**에 사용합니다.



## 출처

- [Eureka](https://coe.gitbook.io/guide/service-discovery/eureka)
- [[MSA] Spring Cloud Eureka에 관하여 - 이론편](https://sabarada.tistory.com/61)

