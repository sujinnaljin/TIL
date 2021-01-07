# Zipkin

![Meet Zipkin: A Tracer for Debugging Microservices – The New Stack](https://thenewstack.io/wp-content/uploads/2016/10/Zipkin.jpg)



- MSA 환경의 **분산 트랜젝션 모니터링** 시스템

- 분산환경에서의 시스템 병목 현상 파악 가능

- 트위터에서 사용하는 오픈 소스

  

## 분산 트렌젝션의 개념

- **분산 트렌젝션**이란 **여러개의 서비스를 걸쳐서 이루어 지는 트렌젝션**이다. 

- 마이크로 서비스 아키텍쳐 (이하 MSA)와 같은 구조에서는 하나의 HTTP 호출이 **내부적으로 여러개의 서비스**를 거쳐서 일어나게 되는데, 그러면 어느 구간에서 병목이 생기는지 **추적하기가 어려**워진다.
- 아래 그림을 보면 클라이언트가 Service A를 호출하고, Service A 가 Service B,D 를, Service B가 Service C를 호출한다. 

![img](https://t1.daumcdn.net/cfile/tistory/99EAB4355AB662612E)



이렇게 **트렌젝션이 여러 컴포넌트의 조합을 통해서 발생**하기 때문에 Jennifer와 같은 **전통적인 APM** (Application Performance Monitoring) 도구를 이용해서 **추적하기가 어렵**기 때문에 **별도의 분산 로그 추적 시스템이라는 것이 필요**하다.



## 작동 원리

그러면 이러한 분산 로그는 어떻게 수집 및 추적하는 것일까? 통상적으로 **Trace**와 **Span** 이라는 개념을 사용한다.

![img](https://t1.daumcdn.net/cfile/tistory/993A8D405AB6626107)





클라이언트가 **서버로 호출한 하나의 호출을 Trace**라고 했을 때, **서비스 컴포넌트간의 호출을 Span**이라고 한다.

각 서비스 컴포넌트들은 **하나의 클라이언트 호출을 추적하기 위해서 같은 Trace Id**를 사용하고, 각 **서비스간의 호출은 각각 다른 Span Id**를 사용한다. 이렇게 함으로써 **전체 트렌젝션 시간을 Trace로 추적**이 가능하고, 각 **서비스별 구간 시간은 Span으로 추적**할 수 있다. 

## 솔루션

이러한 **분산 로그 추적을 위한 솔루션** 중에 오픈소스로는 아래와 같은 것들이 있다.

- 트위터에서 개발된 ZipKin(https://zipkin.io/)
-  Jagger(https://jaeger.readthedocs.io/en/latest/) 
- Opencensus(https://opencensus.io/) 

이러한 분산 로그 추적은 구글의 Dapper 논문을 기초로 디자인 되어 개발되었다. 



# Zipkin

그 중에서, 가장 활성화 되어 있는 오픈소스 중 하나가 Zipkin인데, 오픈 소스 생태계가 활발해서 플러그인이나 부가적인 도구들이 많다. 

전체적인 구조는 다음과 같다.

![img](https://t1.daumcdn.net/cfile/tistory/9922993E5AB662612A)



### 지원 프로토콜

Zipkin으로 추적할 수 있는 분산 트렌젝션은 **HTTP를 기본으로 지원**하고 , 이외에도 많이 사용되는 **리모트 프로토콜**인 **gRPC**를 함께 지원한다.

### 클라이언트 라이브러리 

Zipkin 클라이언트 SDK는 https://zipkin.io/pages/existing_instrumentations 에 있는데, Zipkin에서 공식적으로 지원하는 라이브러는 아래와 같이 C#, Go, Java, Javascript,Ruby,Scala 등이 있다.

![img](https://t1.daumcdn.net/cfile/tistory/99CC343D5AB6626119)

이외에도 오픈 소스 커뮤니티에서 지원하는 라이브러리로 파이썬, PHP등 대부분의 언어가 지원이 가능하다.

**Zipkin 라이브러리**는 **수집된 트렌젝션 정보를 zipkin 서버의 collector 모듈로 전송**한다. 이 때 다양한 프로토콜을 사용할 수 있는데, **일반적으로 HTTP**를 사용하고, **시스템의 규모가 클 경우에는 Kafka 큐를 넣어서 Kafka 프로토콜로 전송**이 가능하다.

### 스토리지

Zipkin 클라이언트 SDK에 의해서 전송된 정보는 스토리지에 저장된다.

사용할 수 있는 스토리지는 다음과 같다

- In-memory
- MySQL
- Cassandra
- Elastic Search

메모리는 별도의 스토리지 설치가 필요없기 때문에 간단하게 로컬에서 테스트할 수 있는 정도로 사용하는 것이 좋고, MySQL은 소규모 서비스에 적절하다. 실제로 **운영환경에 적용하려면 Cassandra나 Elastic Search를 저장소로 사용**하는 것이 바람직하다.

### 대쉬 보드

이렇게 수집된 정보는 대쉬 보드를 이용하여 시각화가 가능하다. Zipkin 서버의 대쉬보드를 사용할 수 있고, **Elastic Search 백앤드를 이용한 경우에는 Kibana를 이용하여 시각화**가 가능하다.

![img](https://t1.daumcdn.net/cfile/tistory/99903A4C5AB662610D)



# [Spring Sleuth](https://github.com/sujinnaljin/TIL/blob/master/Sleuth.md)

**Zipkin 라이브러리** 중에서 주목해서 살펴볼 부분은 **Spring / Java 지원**인데, **Spring에서 Sleuth라는 모듈 이름으로 공식적으로 Zipkin을 지원**하기 때문에, Spring (& Springboot) 연동이 매우 쉽다.

자바 애플리케이션에서 Trace 정보와 Span 정보를 넘기는 원리는 다음과 같다.

![img](https://t1.daumcdn.net/cfile/tistory/995110475AB662611C)



**여러개의 클래스의 메서드들을 거쳐서 트렌젝션이 완성**될때, **Trace 정보와 Span 정보 Context가 유지**가 되어야 하는데, 자바 애플리케이션에서는 쓰레드마다 할당되는 쓰레드의 일종의 전역변수인 **Thread Local 변수에 이 Trace와 Span Context 정보를 저장하여 유지**한다.

분산 트렌젝션은 HTTP나 gRPC로 들어오기 때문에, Spring Sleuth는 **HTTP request가 들어오는 시점과 HTTP request가 다른 서비스로 나가는 부분을 랩핑하여 Trace와 Span Context를 전달**한다.

아래 그림과 같이 HTTP로 들어오는 요청의 경우에는 Servlet filter를 이용하여, Trace Id와 Span Id를 받고 (만약에 이 서비스가 맨 처음 호출되는 서비스라서 Trace Id와 Span Id가 없을 경우에는 이를 생성한다.), 다른 서비스로 호출을 할 경우에는 RestTemplate 을 랩핑하여, Trace Id와 Span Id와 같은 Context 정보를 실어서 보낸다.



![img](https://t1.daumcdn.net/cfile/tistory/995DA3485AB6626102)



HTTP를 이용한 Trace와 Span 정보는 HTTP Header를 통해서 전달되는데

![img](https://t1.daumcdn.net/cfile/tistory/990ACC4D5AB6626126)



위의 그림과 같이 x-b3로 시작하는 헤더들과 x-span-name 등을 이용하여 컨택스트를 전달한다.

이렇게 ServletFilter와 RestTemplate을 Spring 프레임웍단에서 랩핑해줌으로써, 개발자는 별도의 트레이스 코드를 넣을 필요 없이 Spring을 이용한다면 분산 트렌젝션을 추적할 수 있도록 해준다.



## 출처

[Zipkin을 이용한 MSA 환경에서 분산 트렌젝션의 추적 #1](https://bcho.tistory.com/1243)

