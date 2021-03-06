# Micrometer

![micrometerLogo](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQVTqs0OCaGGV2K2xokA6vLnP1LQD971LN7mA&usqp=CAU)
- **모니터링 시스템**을 위한 측정 클라이언트에 대한 **파사드 제공**
- 벤더중립(vender neutral) **API를 이용**해서 **내 코드**를 **시간, 카운트, 계측 할 수 있**는 차원 우선 계측 집합의 **facade**. (dimensional-first metrics collection facade)
- 스프링부트1에 존재했던 카운터, 게이지에 더 풍족한 기본 요소를 추가했는데, 예로 단일 마이크로미터 타이머(Timer)는 **처리량**(throughput), **총 시간**(total time), **샘플의 최대 지연시간**(maximum latency of recent samples), **퍼센테이지**(& 히스토그램)등을 생성 할 수 있다.
- 메트릭 정보를 수집하기 위해서 **약간의 코드만 추가**하면 되도록 설계
- 마이크로미터에 의해서 기록된 어플리케이션 **메트릭 정보**는 주로 **모니터링 용도 또는 알람의 용도**로 사용
- 또한 메트릭 정보의 **이식성을 극대화** (클래스패스나 설정을 통해서 하나 또는 이상의 모니터링 시스템에 메트릭 데이터를 보낼수(export) 할 수 있다. 메트릭 계의 SLF4J)
- 마이크로미터는 다음과 같은 모니터링 시스템을 지원 : Atlas, Datadog, Graphite, Ganglia, Influx, JMX and Prometheus

> Facade 패턴
>
> - 파사드 패턴은 **서브 시스템을 감춰주는 상위 수준의 인터페이스를 제공**함으로써 **코드 중복과 서브 시스템에 대한 직접적인 의존 문제를 해결**한다. 파사드 패턴에서 파사드는 서브 시스템에 접근하여 클라이언트가 원하는 데이터를 제공하는 역할을 한다. 따라서 클라이언트는 파사드에만 의존하게 되며, 서브 시스템의 직접적인 의존이 제거된다. 파사드 패턴을 적용하면 클라이언트는 파사드에만 의존하기 때문에, 서브 시스템의 일부가 변경되더라도 그 여파는 파사드로 한정될 가능성이 높다.
> - 파사드 패턴을 클래스와 비교해보면, 파사드는 마치 서브 시스템의 상세함을 감춰주는 인터페이스와 유사하다. 파사드를 통해서 서브 시스템의 상세한 구현을 캡슐화하고, 이를 통해 상세한 구현이 변경되더라도 파사드를 사용하는 코드에 주는 영향을 줄일 수 있게 된다.
> - 출처 : 개발자가 반드시 정복해야할 객체지향과 디자인 패턴

## 출처

- [Micrometer](https://gunju-ko.github.io/monitoring/2018/06/30/Micrometer.html)
- [springboot2 actuator micrometer](https://bistros.tistory.com/142)
- [Micrometer Application Monitoring](https://micrometer.io/)

