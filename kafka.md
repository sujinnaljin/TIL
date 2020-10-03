# Kafka

- **메세지 큐**의 일종

  메세지 큐 : **메시지 지향 미들웨어**(Message Oriented Middleware: MOM)은 **비동기 메시지를 사용하는 다른 응용프로그램 사이의 데이터 송수신**을 의미하는데 **MOM을 구현한 시스템을 메시지큐**(Message Queue:MQ)라 한다.

- 분산형 스트리밍 플랫폼

- LinkedIn에서 여러 구직 + 채용 정보들을 한곳에서 처리**(발행/구독)할수 있는 플랫폼**으로 개발이 시작

  (발행/구독: pub-sub은 메시지를 특정 수신자에게 직접적으로 보내주는 시스템이 아니고, **메시지를 받기를 원하는 사람이 해당 토픽(topic)을 구독**함으로써 메시지를 읽어 올 수 있다.)

- **대용량의 실시간 로그 처리**에 특화되어 설계된 메시징 시스템, 기존 범용 메시징 시스템대비 **TPS**(Transaction per second)가 매우 우수

- **메시지**를 기본적으로 **메모리에 저장**하는 **기존 메시징 시스템**과는 달리 메시지를 **파일 시스템에 저장** → 카프카 **재시작으로 인한 메세지 유실 우려 감소**

  일반적으로 파일보다 메모리가 성능이 우수한데 카프가가 성능이 좋은 이유는 카프카의 **파일 시스템을 활용한 고성능 디자인**에 있다. 일반적으로 하드디스크는 메모리보다 수백배 느리지만 하드디스크의 **순차적 읽기에 대한 성능은 메모리보다 크게 떨어지지 않는다**고 한다.

- **기본 메시징 시스템**(rabbitMQ, ActiveMQ)에서는 **브로커(Broker)가 컨슈머(consumer)에게 메시지를 push**해 주는 방식인데, **카프카**는 **컨슈머(Consumer)가 브로커(Broker)로부터 메시지를 직접 가져가는 PULL 방식**으로 동작하기 때문에 컨슈머는 **자신의 처리 능력만큼**의 메시지만 가져와 **최적의 성능**을 낼 수 있다. (대용량처리에 특화 되었다는 것은 아마도 이러한 구조로 설계가 되어 가능한 것인듯 함)

  ![img](https://t1.daumcdn.net/cfile/tistory/99715B3A5C3FE33B10)

  

## 카프카 주요 개념

- producer : 메세지 생산(발행)자.

- consumer : 메세지 소비자

  - consumer group : consumer 들끼리 메세지를 나눠서 가져간다. offset 을 공유하여 중복으로 가져가지 않는다.

- broker : 카프카 서버를 가리킴

- zookeeper : 카프카 서버 (+클러스터) 상태를 관리

- cluster : 브로커들의 묶음

- topic : 메세지 종류

- partitions : topic 이 나눠지는 단위

- Log : 1개의 메세지

- offset : 파티션 내에서 각 메시지가 가지는 unique id

  

## 카프카는 어떤식으로 돌아가는가

- zookeeper 가 kafka 의 상태와 클러스터 관리를 해준다.
  ![img](https://taetaetae.github.io/2017/11/02/what-is-kafka/kafka3.png)

- 정해진 topic 에 producer 가 메세지를 발행해놓으면 consumer 가 필요할때 해당 메세지를 가져간다. (여기서 카프카로 발행된 메세지들은 consumer가 메세지를 소비한다고 해서 없어지는게 아니라 카프카 설정`log.retention.hours(default : 168[7일])`에 의해 삭제된다.)
  ![img](https://taetaetae.github.io/2017/11/02/what-is-kafka/kafka4.png)

- partition 개수와 consumer group 개념

  ![img](https://taetaetae.github.io/2017/11/02/what-is-kafka/kafka5.png)

  - 하얀색(consumer-01) : 파티션 개수가 4개인데 비해 컨슈머가 3개, 이렇게 되면 어느 컨슈머가 두개의 파티션을 담당해야하는 상황이 생긴다.
  - 주황색(consumer-02) : 파티션 개수가 4개인데 비해 컨슈머가 5개, 이렇게 되면 하나의 노는(?) 컨슈머가 생기는 상황이 생긴다.
  - 가장 적절한 개수는 정해지지 않았지만 통상 컨슈머그룹의 컨슈머 개수와 파티션 개수를 동일하게 가져가곤 한다.

## 출처

[What is Kafka?](https://taetaetae.github.io/2017/11/02/what-is-kafka)

[카프카(Kafka)의 이해](https://team-platform.tistory.com/11)

