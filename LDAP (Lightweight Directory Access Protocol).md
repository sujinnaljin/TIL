# LDAP (Lightweight Directory Access Protocol)

- 응용 계층 (L7) 인터넷 프로토콜

  ```sh
  ldap://host:port/DN?attributes?scope?filter?extensions
  ```

- 다양한 컴퓨터 **시스템과 애플리케이션의 정보에 대한 접근 권한**을 제공

- 기업 시스템과 보안 서비스에서 **사용자 관리 및 인증에 사용**

- 프로토콜 집합을 사용하여 **정보 디렉터리에 접근**하고 **정보를 검색**

- 디렉터리 서비스는 **이름을 기준으로 대상을 찾아 조회하거나 편집**할 수 있는 서비스. **DNS**도 **도메인 이름으로 IP 주소를 조회**하므로 **디렉터리 서비스**의 일종.


- 디렉터리는 데이터베이스와 유사하지만 특성을 기반으로 하는 더 자세한 정보를 보유
- 디렉터리의 정보는 일반적으로 작성 또는 수정하는 경우보다 **읽는 경우가 많**음
- 용도

  - **사용자, 시스템, 네트워크, 서비스, 애플리케이션 등의 정보**를 **트리 구조**로 저장하여 조회하거나 관리
  - 회사에서 구성원의 **조직도**나 **팀별 이메일** 주소 등도 LDAP 서비스로 관리
  - 특정 영역에서 **이용자명과 패스워드를 확인**하여 **인증**하는 용도로 쓰임
  - 인증이든 무엇이든 트리 구조로 검색하고 편집하기 좋은 데이터들은 LDAP을 많이 사용
  - LDAP은 서버에만 적용되는 프로토콜이 아니라 주소록 관리에 사용되거나 스마트폰 내에서도 LDAP 클라이언트가 포함되어 있음
  - 특정 데이터를 **중앙에서 일괄 관리**하는 일반적인 경우에 사용 (정보를 중앙 집중화하면 한곳에서 관리할 수 있게 되어 작업이 간소화 됨. 사용자 정보가 한곳에서 제공되므로 중복된 정보가 저장되는 일이 줄어들며, 결과적으로 유지 관리해야 할 필요성이 줄어)
  - 유저 권한 관리, 주소록, 조직도, 사용자 정보 관리, 어플리케이션/시스템 설정 정보, 공개 키 인프라스트럭쳐, DHCP나 DNS등의 저장소, 문서 관리, 이미지 저장소, Code 등
- 사용자는 **LDAP 인증**을 통해 단일 로그인 및 비밀번호로 **여러 애플리케이션에 접근**할 수 있음


- 네트워크 상의 디렉토리 서비스 표준인 X.500의 **DAP**(Directory Access Protocol)를 기반으로한 **경량화**(Lightweight) 된 DAP 버전.

  - DAP는 OSI 전체 프로토콜 스택을 지원하며 운영에 매우 많은 컴퓨팅 자원을 필요로하는 아주 무거운 프로토콜
  - LDAP은 DAP의 복잡성을 줄이고 TCP/IP 레이어에서 더 적은 비용으로 DAP의 많은 기능적인 부분을 조작할 수 있도록 설계
- 클라이언트는 다음의 작업을 요청 가능
  - StartTLS — 보안 접속을 위해 LDAPv3 [TLS](https://ko.wikipedia.org/wiki/TLS) 확장을 사용한다
  - Bind — LDAP 프로토콜 버전의 [인증](https://ko.wikipedia.org/wiki/인증) 및 지정한다
  - Search — 디렉터리 엔트리의 검색 및 확인한다
  - Compare — 명명된 엔트리가 주어진 특성 값을 포함하는지 시험한다
  - 새로운 엔트리를 추가한다
  - 엔트리를 지운다
  - 엔트리를 수정한다
  - DN(Distinguished Name)을 수정한다 — 엔트리를 이동하거나 엔트리의 이름을 바꾼다
  - Abandon — 이전의 요청을 중단한다
  - Extended Operation — 다른 오퍼레이션을 정의하기 위해 사용되는 일반적인 오퍼레이션
  - Unbind — 연결을 닫는다 (Bind의 반대가 아님)

# 출처

- [LDAP 인증 제공자 유형](https://help.blackboard.com/ko-kr/Learn/Administrator/SaaS/Authentication/Implement_Authentication/LDAP_Authentication_Provider_Type)
- [LDAP](https://ko.wikipedia.org/wiki/LDAP)
- [알아두면 쓸데있는 LDAP](https://www.samsungsds.com/kr/insights/ldap.html)
- [[LDAP] 개념 잡기](https://yongho1037.tistory.com/796)

