# Prometheus

![Prometheus Monitoring Pros and Cons | NexClipper](https://www.nexclipper.io/wp-content/uploads/2020/06/57307750-696bb980-70e5-11e9-9b0b-73ad88bde6a3.png)

- **이벤트 모니터링 및 경고** 에 사용되는 오픈소스 **모니터링 솔루션** 

- 크게 **Prometheus 서버**와, **메트릭 정보를 export**할 **export node**로 구성 된다.

- **Prometheus는 Pull 방식**을 사용하여, 서버에 클라이언트가 떠 있으면 **중앙 서버(prometheus server)가 주기적으로 클라이언트에 접속해서 데이터(메트릭)를 가져오는 방식**을 취한다.

  참고로 **대부분의 모니터링 도구**가 **Push** 방식 즉, 각 서버에 클라이언트를 설치하고 이 **클라이언트가 메트릭 데이터를 수집해서 서버로 보내**면 서버가 모니터링 상태를 보여주는 방식을 취한다.

- 간단한 **텍스트 형식으로 메트릭을 쉽게 노출** 가능하며, 데이터 모델은 key-value 형태로 레이블을 집계한 후 , Grafana같은 대시 보드 시스템에서 그래프로 쉽고 간단하게 Dashboard 를 만들 수 있다.

- **장점** : 일정 주기(default 15s)로 발생하는 metric을 수집하는 **pull 방식의 구조**를 채택함으로써 **모든 메트릭에 대한 데이터를 중앙 서버로 보내지 않아**도 된다. (**부하 감소**)

- **단점** : 모든 **메트릭을 전송하지 않**기 때문에 사실상 **"추이"를 보는데는 좋**지만 APM(Application Performance Monitoring)과 같이 발생한 **모든 로그를 추적**하고 문제가 발생했을 때 이를 검색해서 어떤 일이 있었는지의 원인을 밝히고자 했을때는 **적합하지 않**다

  

## Prometheus Architecture

![Image for post](https://miro.medium.com/max/2816/1*tHxrGpEUte5iz8IVKAhyfQ.png)

- **Prometheus server**

  - **Retrival** : **메트릭을 수집할 대상 서버**에 접근해서(HTTP) **메트릭을 가져오**거나, 아니면 **Pushgateway** 를 통해서 **접근할 수 없는 곳에 있는 데이터**(inner server, firewall 내부의 metric 등)**를 가져오**는 등의 역할을 하게 된다

  - **TSDB(Time-series Database)** : 가져온 데이터를 저장하고, 시간의 흐름에 따라 조회를 할 수 있어야 하므로 **시-계열 데이터(time-series) 를 저장할 수 있는 저장소**가 prometheus 내부에 구현이 되어 있다.

    프로메테우스의 메트릭은 `메트릭명{필드1=값, 필드2=값} 샘플링데이터` 과 같은 형식으로 발생 하게 되고, 이를 **text/html 방식으로 특정 url(대부분 /metrics)로 export**를 해두게 되면, **prometheus 서버가 이를 긁어가서 데이터를 저장**하는 구조이다.

  - **HTTP Server** : prometheus에 저장된 **데이터를 조회하기 위해**서는 내부적으로 **HTTP 서버가 필요**하다. 따라서 prometheus는 데이터를 가져가기 위한 프로토콜로 HTTP REST API를 제공하고, 직접 API를 통해 데이터를 가져가던지, Web UI 대시보드에서 데이터를 조회한다던지, Grafana를 통해 더욱 자세하고 깔끔한 데이터 시각화를 할 수 있다

- **Service discovery** : Prometheus는 기본적으로 **모니터링 대상 목록**을 유지하고 있으며, 대상에 대한 ip나 기타 접속 정보를 설정 파일에 주어서 **모니터링 정보를 가져오는 방식**을 사용한다.

  이러한 환경을 대처하기 위해 **서비스 디스커버리**를 사용하지만, **오토스케일링**을 하는 환경에서는 **ip가 동적으로 변경**되는 경우가 많기 때문에 , 모니터링 대상이 등록되어 있는 **저장소에서 목록을 받아서 그 대상을 모니터링**하는 형태를 취한다. ex) kubelet

- **Push gateway** : Pushgateway는 쉽게말해 Proxy Forwarding을 해서 **접근할 수 없는 곳에 데이터가 존재하는 경우에 사용**할 수 있는 대안이다. (사내망에 데이터가 있어서 외부에서 scrape를 하고싶어도 접근이 안되는 경우 등)

  application 이 **pushgateway 에 메트릭을 push**한 후, **prometheus server** 가 pushgateway 에 접근해 **metric 을 pull** 해서 오는 방식으로 동작한다.

- **Exporter & job** : Exporter 는 Prometheus 에게 **메트릭을 가져가도록 특정 Service 에 metric 을 노출하게 하는 Agent** 라고 이해하면 편하다.

  Exporter 는 서버 상태를 나타내는 Node exporter , SQL Exporter 등 다양한 커스텀 exporter 이 개발되어 사용되고 있으며, 이러한 Exporter를 사용하여 metric 을 Prometheus 에 긁어가도록 할 수 있다.

- **Alert manager** : Alert manager 는 **metric 에 대한 어떠한 지표** 를 걸어놓고 그 규칙을 **위반하는 사항에 대해 알람**을 전송하는 역할을 한다. (slack, hipchat 등을 통해 알람)

- **Data visualiztion** : Data visualiztion은 다양한 모니터링 **Dashboard 를 위한 visualization** 을 제공한다. 주로 Prometheus 가 수집한 데이터에 대한 외부 시각화 툴 및 api를 제공하는 역할을 한다.

  

## 출처 

[Prometheus 를 알아보자](https://gompangs.tistory.com/entry/Prometheus-를-알아보자)

[Prometheus 를 이용한 모니터링 — Part 1](https://medium.com/@tkdgy0801/prometheus-%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-part-1-69de3e87d427)



