# CDN(Contents Delivery Network)

- **지리,물리적**으로 떨어져 있는 사용자에게 **컨텐츠를 더 빠르게** 제공할 수 있는 기술. -> 느린 응답속도 / 다운로딩 타임 극복

- **서버 자체를 여러곳**에 두고 이용자가 요청했을때 제일 **근접한 서버**에서 처리함으로써 지연되는 시간을 줄여 준다.

- 사용자와 가까운 곳에 위치한 **Cache Server에 해당 Content를 저장(캐싱)** 하고 Content 요청시에 Cache Server가 응답을 준다

  

![img](https://t1.daumcdn.net/cfile/tistory/99EA983C5C5304ED21)

## CDN의 작동원리

1. **최초 요청**은 서버로 부터 컨텐츠를 가져와 고객에게 전송하며 동시에 **CDN 캐싱장비에 저장**한다.
2. 두번째 이후 모든 요청은 CDN 업체에서 지정하는 **해당 컨텐츠 만료 시점까지 CDN 캐싱장비**에 저장된 컨텐츠를 전송한다.
3. **자주 사용**하는 페이지에 한해서 CDN 장비에서 **캐싱**이 되며, 해당 컨텐츠 **호출이 없을 경우 주기적으로 삭제**된다.
4. 서버가 **파일을 찾는 데 실패**하는 경우 CDN 플랫폼의 **다른 서버에서 콘텐츠를 찾아** 엔드유저에게 응답을 전송한다.
5. 콘텐츠를 **사용할 수 없**거나 콘텐츠가 **오래된 경우**, CDN은 서버에 대한 요청을 **프록시로 작동** 하여 향후 요청에 대해 응답할 수 있도록 **새로운 콘텐츠를 저장**한다.



## CDN의 필요기술

**1. Load Balance**

- 사용자에게 콘텐츠 전송 요청(Delivery Request)을 받았을 때, **최적의 네트워크 환경을 찾아 연결**하는 기술, [GSLB(Global Server Load Balancing)](https://github.com/sujinnaljin/TIL/blob/master/GSLB(GlobalServerLoadBalancing).md)이라고도 한다. GSLB는 DNS(도메인 이름을 IP주소로 변환하는 서비스) 서비스의 발전된 형태.

- **물리적**으로 가장 **가깝거나 여유 트래픽**이 남아 있는 곳으로 접속을 유도하는 기술이다. 

**2. 컨텐츠를 배포하는 기술**

- 컨텐츠의 삭제나 수정이 일어났을 때 이를 관리할 수 있는 기술이 필요하다.

**3. CDN의 트래픽을 감지하는 기술**

- 통계자료를 고객에게 제공하기 위해 필요하다.

- **트래픽을 분산**하기 위해서 필요하다



## **CDN 캐싱 방식**

**1. Static Caching**

Origin Server에 있는 Content를 운영자가 **미리 Cache Server에 복사**

**미리 복사**해 두기 때문에 사용자가 Cache Server에 Content를 요청시 **무조건 Cache Server**에 있다.

대부분의 국내 CDN에서 이 방식을 사용( ex. NCSOFT 게임파일 다운로드 등)

**2. Dynamic Caching**

Origin Server에 있는 Content를 운영자가 **미리 Cache Server에 복사하지 않음**

사용자가 **Content를 요청시** 해당 Content가 없는 경우 Origin Server로 부터 다운로드 받아 전달 (Content가 있는 경우는 캐싱된 Content 사용자에게 전달)

각각의 Content는 일정 시간 이후 Cache Server에서 삭제될 수도 있다. (계속 가지고 있을 수도 있음)



## 출처

- [CDN(Contents Delivery Network) 이란?](https://goddaehee.tistory.com/173)

