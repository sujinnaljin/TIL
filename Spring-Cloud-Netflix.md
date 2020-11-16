# Spring Cloud Netflix

- **Spring Boot 어플리케이션을 위한 Netflix OSS(Open Source Software) 통합을 제공**합니다
- battle-tested를 거친 Netflix component를 통해 **대규모 분산 시스템을 구축**할 수 있습니다.

## 제공 패턴

- **Service Discovery (Eureka)**
  - **서비스 등록 및 탐색**
  - Eureka 인스턴스를 등록할 수 있으며, client는 Spring이 관리하는 빈을 사용하여 탐지할 수 있다.
  - 내장된 Eureka Server는 선언적 java config를 통하여 생성될 수 있다.
- **Circuit Breaker (Hystrix)**
  - Hystrix clients는 간단한 어노테이션 기반으로 구축될 수 있다.
  - 선언적 java config로 내장된 Hystrix dashboard
- **선언적 REST Client (Feign)**
  - **REST API 호출을 용이**하게 해주는 HTTP Client
  - Feign은 JAX-RS 또는 Spring MVC 어노테이션으로(선언적) 인터페이스를 동적으로 구현한다.
- **Client Side Load Balancer (Ribbon)**
  - zuul에 내장된 **로드 밸런서**
- **External Configuration**
  - **환경 설정 외부화** 담당
  - Spring 환경에서 Archaius로 연결 (Spring Boot를 사용하여 Netflix 구성 요소를 설정 가능)
- **Router and Filter**
  - Zuul 필터의 자동 등록 및 Reverse Proxy 생성 설정 접근에 대한 간단한 규칙
-  **Proxy & Gateway (Zuul)**





![img](https://t1.daumcdn.net/cfile/tistory/99FE3B3E5C5F1FD60B)

spring-cloud-netflix 생태계는 다음과 같다.

- **모든 요청은 Zuul(API-Gateway)에서 처리**되며, URI에 맞는 서비스를 **라우팅**한다. 만약 **동일한 서비스가 여러 개의 인스턴스**에 있는 경우에는 **로드밸런싱을 통해 트래픽**을 나눈다. 이는 **Zuul에 내장된 Ribbon이 하는 기능**이며, Default는 라운드로빈 방식에 의해 분산이 된다.
- **모든 서비스는 유레카 서버에 등록**된다.
- **Eureka**는 URL을 해석하여 해당 서비스의 **서버 정보를 Zuul에게 전달**한다. 그럼 이를 통해서 **Zuul이 해당 서버에 라우팅**을 한다.
- 또한 Filter 기능을 통하여 로깅, 인증, 모니터링, CORS 정책 같은 서비스 간의 공통된 기능을 처리할 수 있다.
- 인스턴스간의 모든 호출(서비스와 서비스, gateway와 서비스 ... 등) 사이에는 **Hystrix Circuit breaker 패턴이 적용**된다.



## 출처

- [Spring Cloud Netflix (1) - OverView](https://swiftymind.tistory.com/107)

- [[Spring] Spring Cloud — Config Server를 활용한 환경구성의 외부화](https://medium.com/@yongkyu.jang/spring-cloud-config-server-f1e390f18cfc)

- [[MSA\] #6 Spring Cloud Netflix](https://alwayspr.tistory.com/22)

