# SSH

## SSH?

- **Secure Shell** (보안 껍데기(?))의 약자. 

- 네트워크 통신에 사용되는 **telnet을 보완**하기 위해 개발된 소프트웨어

  - telnet은 서로 다른 두 컴퓨터를 연결하여 파일을 주고 받을 수 있는 소프트웨어
  - telnet은 데이터를 전송하는 과정에서 **암호화를 이용하지 않기** 때문에 패킷이 유출될 경우 비밀정보까지 노출 가능성 있음

- 서버에 접속 할 때 **비밀번호 대신 key를 제출**하는 방식

- 비밀번호 보다 **높은 수준의 보안**을 필요로 할 때, 혹은 로그인 없이 **자동으로 서버에 접속** 할 때 사용

- 혹은 GitHub 계정이 2FA(Two-Factor Authentication)에 의해 **이중 인증이 필요** 할 때

  

## SSH가 동작하는 방식

- SSH Key는 **공개키**(public key)와 **개인 키**(private key)로 이루어진다
- **공개키**는 외부에서 나에게 메시지를 보낼 때 사용하는 **암호화** 키
- **개인키**는 외부에서 나에게 보낸 메시지를 **복호화** 할 때 사용하는 키
- 개인 키는 **로컬 머신**에, 공개키는 **리모트 머신**에 위치 (로컬 머신은 SSH Client, 원격 머신은 SSH Server가 설치된 컴퓨터를 의미)
- SSH 접속을 시도하면 SSH Client가 로컬 머신의 비공개키와 원격 머신의 비공개키를 비교해서 둘이 일치하는지를 확인

![img](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/432/1213.gif)

### View of Github

- **Github**은 우리의 컴퓨터와 연결될 수 있도록 **SSH 기능**을 제공
- 즉, **github에게 '내 공개키'를 전달**하기만 한다면 github이 나에게 보내는 모든 **메시지**는 **'개인키'를 가지고 있는 나만이 이해**할 수 있는 것
- 이로써 github에게 내가 프로젝트의 **주인임을 인증** 가능
- 따라서 이 기능을 이용하면 매번 **비밀번호를 입력하지 않고도 '인증'** 이 가능
-  SSH는 단순히 '인증'을 위한 수단이 아니고 **데이터를 안전하게 전송하기 위한 프로토콜**임을 꼭 기억하시길 바랍니다..



![img](http://postfiles8.naver.net/MjAxOTA2MTdfODIg/MDAxNTYwNzcyMzQzODgw.BSQSBu66iAuc4T3Bv_cwWnbPt48Ngcfdbv2IBmw_N3Ig.hWPF3xFnEPvR_5vSgGK6W1PpKonMq94PfTIwA8gYvPog.PNG.pjok1122/image.png?type=w966)

## SSH Key 만들기

- Unix 계열(리눅스, 맥)에서는 **ssh-keygen**이라는 프로그램을 이용하여 생성 가능

  `ssh-keygen -t rsa -b 4096 -C "yourEmail@example.com`

  - t rsa는 rsa라는 암호화 방식으로 키를 생성한다는 의미
  - SSH 키는 키 크기가 2048비트 또는 4096비트인 RSA 키여야 한다. 해당 경우는 4096으로 지정

- 생성 후 아래와 `~/.ssh` 에서 아래와 같은 세가지 파일 확인 가능

  -  authorized_keys -  id_rsa.pub 키의 값을 저장
  -  id_rsa - 개인키, 타인에게 노출되면 안되는 **private key**. 본인의 컴퓨터 내부에 저장하게 되어 있으며, 이 Private Key를 통해 암호화된 메시지를 **복호화** 할 수 있다.
  -  id_rsa.pub - 공개되어도 비교적 안전한 **public key**. Public Key를 통해 메시지를 전송하기 전 **암호화**를 하게 된다.

## github 연동

- Setting에서 **SSH 공개키(id_rsa.pub) 등록** 후 **clone with SSH**
- 공개키 확인 방법
  
  `cat(또는 vi) ~/.ssh/id_rsa.pub`

  ![img](https://blog.kakaocdn.net/dn/MtOh1/btqEbQT4L2l/R52xk8mdVyAWcpklkHnEfk/img.png)

  

  ![img](https://blog.kakaocdn.net/dn/AKW3Z/btqEfqeF2yb/Gz5Lhkn7kA9ooPG9mghOk0/img.png)

  

## 출처

- [SSH Key - 비밀번호 없이 로그인](https://opentutorials.org/module/432/3742)

- [[Git (7)] Github 비밀번호 입력 없이 pull/push 하기(github ssh key 설정)](https://goddaehee.tistory.com/254)

- [버전 관리 시스템 / Git \] SSH 이용하기](http://blog.naver.com/pjok1122/221564348763)

