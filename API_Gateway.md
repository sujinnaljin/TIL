# API Gateway
- **API 클라이언트와 서비스 사이에 프록시처럼 위치**하여, 클라이언트로 부터 온 **API 요청을 받아서 서비스로 라우팅** 하는 역할을 한다. 
- API 게이트웨이는 **마이크로서비스의 중요 컴포넌트**이다.
-  근래에 마이크로 서비스가 각광을 받으면서 언급되기 시작하고 있긴 하지만, 사실 API 게이트웨이라는 기술 자체는 오래된 기술이다.


여러가지 기능이 있겠지만, 크게 아래와 같이 5가지로 나눠볼 수 있다.
## 1. 인증
API를 호출할때, **API 호출에 대한 인증**을 수행한다. 서버간의 API 통신일 경우에는 간단하게 **API 키**를 사용하는 방법등이 있고, 다수의 클라이언트 (모바일 단말, 웹 자바스크립트 클라이언트)의 경우에는 사용자 계정 인증 후에, **JWT 토큰**을 발급하는 방식이 있다. 

## 2. 모니터링 & 로깅
API 호출에 대한 모니터링을 통해서 API 별 **응답시간,  장애율, 호출 횟수를 모니터링** 할 수 있게 하고, 경우에 따라서 **개별 API 호출을 로깅**한다. 

### Metering & Charging
API를 **대외로 서비스 하는 경우, API의 호출량을 모니터링** 해야 하는데, 이 양을 기반으로 해서 **API 서비스 호출 횟수를 통제**하거나 또는 유료 API의 경우에는 **과금**을 해야 하기 때문에, 대외로 서비스를 하는 오픈 API 플랫폼인 경우에는 이러한 기능이 필요하다. 

## 3. 메시지 플로우 컨트롤
메시지 플로우 컨트롤은 **클라이언트로 부터 들어온 메시지의 흐름을 바꾸는 방법**인데, 예를 들어 **같은 API라도 버전에 따라서 구버전과 새버전으로 트래픽을 9:1로 라우팅**하는 카날리 배포 방식이라던지, 들어온 API를 **클라이언트의 위치에 따라 미국이나,한국에 있는 서비스로 라우팅**을 하는 등의 로직을 구현할 수 있다. 

### Limiting (throttling)
조금 더 고급화된 구현방법으로는 Limiting 기법이 있는데, 특정 API에 대해서 **정해진 양 이상의 트래픽을 받지 못하도록** 하여, **서비스를 보호**하거나 또는 유료 API 서비스인 경우 **QoS (Quality Of Service)를 보장**하기 위해서 특정 양까지만 트래픽을 허용하도록 컨트롤할 수 있다.

## 4. 메시지 변환
메시지 변환은 고급 기능 중의 하나인데, **헤더에 있는 내용을 메시지 바디로** 붙이거나, **다른 API를 호출해서 응답메시지를 두 API를 합해서 응답**을 해주는 기능등을 구현할 수 있다.

### 프로토콜 변환
그외에도 서로 다른 프로토콜에 대한 변환이 가능한데, 예를 들어 클라이언트로 부터는 gRPC로 호출을 받고, API서버로는 REST로 호출한다던가. SOAP/XML 기반의 메시지를 REST로 변환하는 등의 기능 구현이 가능하다. 

### 호출 패턴 변환
API 서버가 큐를 기반으로한 비동기 메시지 처리 패턴일때, API 게이트 웨이에서 이를 동기화 처리 패턴으로 바꿔서, 클라이언트 입장에서는 동기로 호출을 하면, API 서버에서는 비동기로 호출하는 형태와 같이 호출 패턴을 변화시킬 수 있다. 

## 5. 오케스트레이션(매시업)
API 클라이언트가 **한번 호출을 할때, 동시에 여러개의 API를 호출하도록 API 게이트웨이에서 호출**을 해주는 방식이다. 예를 들어 API 클라이언트가 “상품 구매" 라는 API를 호출 하였을때, API 게이트웨이가 “상품 구매" API 서비스를 호출하고, “상품 추천" 이라는 API도 호출해서, 클라이언트로 구매 완료 메시지와 함께 추천 상품 리스트를 한번에 리턴하는 방식이다. 



![](https://t1.daumcdn.net/cfile/tistory/99AF2C495DD2B8472A)

**출처** [API 게이트 웨이 & Google Cloud Endpoints](https://bcho.tistory.com/1365?category=731548)