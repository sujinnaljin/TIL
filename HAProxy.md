# HAProxy

- High availability Proxy
-  기존의 하드웨어 스위치를 대체하는 **소프트웨어 로드 밸런서**
- 네트워크 스위치에서 제공하는 L4, L7 기능 및 로드 밸런서 기능을 제공

## HAProxy 동작 방식

- HAProxy는 기본적으로 **reverse proxy** 형태로 동작
- **reverse proxy**는 실제 서버 요청에 대해서 **서버 앞 단에 존재**하면서, 서버로 들어오는 **요청을 대신 받아**서 서버에 전달하고 요청한 곳에 그 결과를 다시 전달하는 것이다.
- 우리가 **브라우저에서 사용하는 proxy**는 **클라이언트 앞에서 처리**하는 기능으로, **forward proxy**라 한다. 

HAProxy의 동작 흐름은 다음과 같다.

1. 최초 접근 시 서버에 **요청 전달**
2. 응답 시 **쿠키(cookie)에 서버 정보** 추가 후 반환
3. 재요청 시 proxy에서 **쿠키 정보 확인** > **최초 요청 서버로 전달**
4. 다시 접근 시 쿠키 추가 없이 전달 > 클라이언트에 쿠키 정보가 계속 존재함(**쿠키 재사용**)

![haproxy1](https://d2.naver.com/content/images/2015/06/helloworld-284659-1.png)

## HAProxy HA(High availability) 구성

- HAProxy는 기본적으로 VRRP(Virtual Router Redundancy Protocol)를 지원
- 소프트웨어 기반의 솔루션이기 때문에 HAProxy가 설치된 **서버에서 문제가 발생**하면 하드웨어 **L4보다는 불안정**할 수 있다. 
- 따라서 HA 구성으로 master HAProxy에 문제가 생기는 경우에도 **slave HAProxy에서 서비스가 원활하게 제공**될 수 있는 구성을 알아보겠다.
  - 가상 IP 주소를 공유하는 **active HAProxy 서버와 standby HAProxy 서버가 heartbeat를 주고 받으면서 서로 정상적으로 동작하는지 여부를 확인**한다. 
  - active 상태의 서버에 문제가 발생하면 **standby HAProxy가 active 상태로 변경**되면서 **기존** active HAProxy의 **가상 IP 주소**를 가져오면서 서비스가 무정지 상태를 유지한다. 
  - 다만 1초 정도의 순단 현상은 발생할 수 있다.

![haproxy2](https://d2.naver.com/content/images/2015/06/helloworld-284659-2.png)


## 출처
- [L4/L7 스위치의 대안, 오픈 소스 로드 밸런서 HAProxy](https://d2.naver.com/helloworld/284659)
