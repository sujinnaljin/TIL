# SSL 인증서와 iOS 13 이상에서 인증서에 대한 요구 사항
## SSL 인증서
- **SSL 인증서**는 클라이언트와 서버간의 **통신을 제3자가 보증**해주는 전자화된 문서

- 해당 웹 사이트가 해킹에 신뢰있는 사이트인지, 안전한 통신을하는 사이트인지를 구분하기 위해 **누군가**가 이 사이트가 **신뢰가 있는 사이트라고 인증**을 해주기 위한 **인증서**

- 인증서에는 인증서 **소유자의 email, 소유자 이름, 인증서 용도, 인증서 유효기간, 발행기관, public key** 등이 포함되어 있음

  인증서에 포함된 **서버의 공개키**는 클라이언트와 서버가 데이터를 주고 받을기 위해 필요한 **대칭키를 주고 받을 때 사용**. (클라이언트에서 **대칭키**를 생성하고, 이를 서버로 부터 받은 **공유키를 이용해 암호화**해서 보냄. 서버는 **비공개 키로** 해당 데이터를 **복호화**해서 **대칭키**를 받음. 이제 해당 **대칭키**로 내용들을 **암호화 / 복호화** 하며 주고 받음) -> 더 자세한 설명은 하단 SSL 쪽에 기재

- 인증서에 **서명한 사람을 신뢰**한다면, **서명된 인증서 또한 신뢰**할수 있음. **인증서가 다른 인증서에 서명**을 해주며 트리구조로 되어 있음

  **브라우저**는 내부적으로 **CA의 리스트를 미리 파악**하고 있음 (브라우저의 **소스코드 안에 CA의 리스트**가 들어있음) 브라우저가 미리 파악하고 있는 **CA의 리스트에 포함**되어야만 **공인된 CA**가 되는 것. 

  CA의 리스트와 함께 각 [CA의 공개키](https://opentutorials.org/course/228/4894#public)를 브라우저는 이미 알고 있음
    * 공인된 CA (인증기관)가 제공하는 인증서를 사용해서 ssl 적용
      
      <img width="246" alt="스크린샷 2022-03-25 오후 1 44 04" src="https://user-images.githubusercontent.com/20410193/160064848-6f8a4516-758f-424a-97b6-f0944146384a.png">

    * 사설 CA (인증기관)에서 발급받은 인증서를 사용하여 ssl 을 적용
      
      <img width="305" alt="image-20220325134400475" src="https://user-images.githubusercontent.com/20410193/160064870-fc3f03ff-05c6-4716-afa3-e807334cb059.png">

    * ssl 미적용
      
      <img width="224" alt="image-20220325134423726" src="https://user-images.githubusercontent.com/20410193/160064882-34fd77e4-212d-4f11-b1c8-e28a973012c0.png">
* 근데 가끔 인증서가 말썽을 부려서 특정 단말이나 os 버전에서 접속 안된다던지 그런 경우도 있다고,,!

### SSL 인증서 종류
* 인증서는 여러가지 종류가 있는데 **인증기관(CA)의 심사 수준에 따라 DV, OV, EV 로 구분**되고 각 심사수준은 차이가 있으며, 인증서 발급에 필요한 정보를 요구함. 
* SSL 인증서 발급 기준은 **도메인 * 서브도메인** 갯수. 
* 하지만 관리측면에서 와일드 카드 SSL 인증서를 이용해서 하나의 도메인 앞에 서브 도메인을 무제한으로 사용할 수도 있음.

  > koreassl.com 도메인 기준으로 koreassl.com 을 포함한 **2차 도메인 *.koreassl.com 범위내의 보안통신이 가능**
  >
  > db.m.koreassl.com 경우처럼 **3차 도메인**에 해당된다면 *.koreassl.com 의 와일드카드 인증서 범위에 해당하지 않으므로 *.m.koreassl.com **와일드카드 인증서를 추가로 신청** 필요
  
  <img width="925" alt="image" src="https://user-images.githubusercontent.com/20410193/160065066-08e466dd-87c8-478c-8d9f-5b5272cb68ac.png">


#### DV(Domain Vailidation) : 도메인 유효성 검사

- **도메인 소유권 심사**를 통해 발급되는 가장 쉽고 빠른 SSL 인증서.

- 도메인 소유정보만 확인되면 5분안에 발급되므로 누구나 쉽게 발급 가능.

- 도메인 유효성만 검사하기 때문에 **주체명(도메인)만 표시**됨. 회사 정보를 확인하지 않고, 인증서에 표시되지 않음

- 신뢰성에서는 **OV 및 EV**와 비교되지만 **암호화 알고리즘은 동일**하기 때문에 보안통신에서는 전혀 차이가 없음

- DV 인증서는 고객의 **신뢰도가 높지 않**기 때문에, 민감한 정보를 다루는 사이트에는 적합하지 않음

- Certificate Policy: Policy Identifier=2.23.140.1.2.1

  

![img](https://cert.crosscert.com/wp-content/uploads/2019/12/dv1.png)

#### 2. OV (Organization Validation) : 조직 유효성 심사

- **비즈니스의 적법성 검증**과 **도메인 소유권 심사**를 통해 발급되는 인증서

- 소속되어 있는 회사(조직) 정보를 추가로 인증하는 방법

- 도메인 소유정보와 소속되어 있는 회사(조직)의 기본 정보만 확인되면 발급까지 하루에서 3일정도 소요

  회사(조직)의 실체인증 위해 영문사업자등록증 / 전화번호 검색사이트(114.co.kr)에서 지역번호로 시작하는 대표번호 등록(1588등 불가) 또는 [D-U-N-S Number](https://www.koreassl.com/support/faq/DUNS-Number-가-뭔가요) / [LEI Code](https://www.koreassl.com/support/faq/LEI-Code-가-뭔가요) 등록이 필요

- 검증된 회사정보는 인증서에 표시되며, 사이트의 소유권을 확인할 수 있음.

  - 도메인 소유권 확인
  - 조직(Organization) 확인
  - 조직(회사) 담당자의 재직증명 확인

- DV 인증서와 비교하여 고객에게 사이트에 대한 신뢰성을 확보 가능

- 네이버 / 카카오 / 삼성 등 대기업에서 많이 사용

- Certificate Policy: Policy Identifier=2.23.140.1.2.2

![img](https://cert.crosscert.com/wp-content/uploads/2019/12/OV.png)

#### 3. EV (Extended Validation) : 확장 유효성 심사

- DV, OV 보다 까다로운 검증을 통해 **기업의 실존성을 강화**한 인증서

- OV 검증 + 확장적인 검증이 필요한 인증 방법

- 도메인 소유정보 + 소속되어 있는 법인회사(조직)의 실체확인 + 인증기관에 따라 법인 2년 또는 3년 이상 운영 조건 등 까다로운 조건들이 충족되어야 발급이 가능하며 발급까지 최대 3주까지도 소요

  OV와 마찬가지로 회사(조직)의 실체인증 위해 영문사업자등록증 / 전화번호 검색사이트(114.co.kr)에서 지역번호로 시작하는 대표번호 등록(1588등 불가) 또는 [D-U-N-S Number](https://www.koreassl.com/support/faq/DUNS-Number-가-뭔가요) / [LEI Code](https://www.koreassl.com/support/faq/LEI-Code-가-뭔가요) 등록이 필요

- **최상급의 신뢰성**을 가진 EV 인증서는 발급조건이 까다롭기 때문에 **금융권 / 공공기관 / 카드사** 등에서 많이 사용.

- EV인증서는 브라우저에서 도메인 소유회사의 이름이 표시되며, Green Address Bar를 통해 안전한 사이트임을 확인
  - 도메인 소유권 확인
  - 조직(Organization) 확인
  - 조직(회사) 담당자의 재직증명 확인
- 발급받은 SSL인증서의 소유를 표시해주며 조직 및 조직 담당자의 확인을 해주기 때문에 **가장 높은 신뢰성**
- Certificate Policy: Policy Identifier=2.23.140.1.1

![img](https://cert.crosscert.com/wp-content/uploads/2019/12/EV.png)

### **인증서**가 **서버의 신뢰성을 보장**하는지 **메커니즘**

1. **클라이언트**가 **서버에 접속**할 때 서버는 제일 먼저 **인증서를 제공**

2. 브라우저는 이 **인증서를 발급한 CA**가 자신이 **내장한 CA의 리스트에 있는지**를 확인

3. 확인 결과 서버를 통해서 다운받은 인증서가 내장된 **CA 리스트에 포함**되어 있다면 **해당 CA의 공개키를 이용해서 인증서를 복호화**

   == **CA의 공개키를 이용**해서 **인증서를 복호화** 할 수 있다는 것은 이 인증서가 **CA의 비공개키에 의해서 암호화** 된 것을 의미

   == 해당 **CA의 비공개 키를 가지고 있는 CA**는 해당 CA 밖에는 없기 때문에 서버가 제공한 **인증서가 CA에 의해서 발급**된 것이라는 것을 의미

   == CA에 의해서 발급된 인증서라는 것은 접속한 사이트가 **CA에 의해서 검토**되었다는 것을 의미

   == CA의 검토를 통과했다는 것은 **해당 서비스가 신뢰** 할 수 있다는 것을 의미

### ssl 인증서 **발급 절차**

1. 사이트에서 **개인키(Private Key)** 를 생성.
2. 사이트의 정보가 담긴 **인증 서명 요청서(CSR)** 를 작성.
3. CSR을 **인증기관(CA)** 으로 전송하여 **인증서(CRT)** 발급을 요청.
4. 해당 **인증기관** 에서는 신청서의 내용을 토대로 사이트를 **검증**.
5. 사이트의 정보와 **공개키가 담긴 인증서**를 인증기관의 **비공개키로 암호화하여 발급**.

- 발급된 인증서를 **다운로드** 받은 후 이런식으로 **인증서 경로 지정**할 수 있음

  ```
  <VirtualHost xxx.xxx.x.x:443>
  	DocumentRoot /var/www/coolexample
  	ServerName coolexample.com www.coolexample.com
  		SSLEngine on
  		SSLCertificateFile /path/to/coolexample.crt
  		SSLCertificateKeyFile /path/to/privatekey.key
  		SSLCertificateChainFile /path/to/intermediate.crt
  </VirtualHost>
  ```

### iOS 13 및 macOS 10.15의 신뢰할 수 있는 인증서에 대한 요구 사항

- 모든 **TLS 서버 인증서**는 다음과 같은 iOS 13 및 macOS 10.15의 **새로운 보안 요구 사항을 준수**해야 함

  - **RSA 키**를 사용하는 TLS 서버 인증서 및 발급 CA는 **2048 비트 이상의 키 크기**를 사용해야 함. 2048 비트보다 작은 RSA 키 크기를 사용하는 인증서는 TLS가 더 이상 신뢰하지 않음.

  - TLS 서버 인증서 및 발급 CA는 **서명 알고리즘**에서 **SHA-2 집합의 해시 알고리즘**을 사용해야 함. SHA-1 서명된 인증서는 TLS가 더 이상 신뢰하지 않습니다.

  - TLS 서버 인증서는 인증서의 주체 대체 이름(Subject Alternative Name) 확장자에 **서버의 DNS 이름을 제공**해야 함. 인증서의 CommonName에 있는 DNS 이름은 더 이상 신뢰되지 않음.

- 또한 2019년 7월 1일 이후에 발급된 모든 TLS 서버 인증서(인증서의 NotBefore 필드에 표시된 대로)는 다음 지침을 따라야 함

  - TLS 서버 인증서에는 **id-kp-serverAuth OID가 포함**된 EKU(ExtendedKeyUsage) 확장자가 있어야 함

  - TLS 서버 인증서의 **유효 기간은 825일 이하**여야 함(인증서의 NotBefore 및 NotAfter 필드에 표시됨).

- 이러한 새로운 **요구 사항을 위반하는 TLS 서버에 연결**하는 경우, 연결되지 않으며 iOS 13 및 macOS 10.15의 Safari에서 **네트워크 오류, 앱 오류**가 발생하거나 **웹 사이트가 로드되지 않을 수** 있음

## SSL

- SSL 연결은 인증되지 않은 사용자의 방해로부터 각 **방문(세션) 중에 교환**된 중요한 **데이터**(예: 신용카드 정보)를 **보호**

  <img width="506" alt="image" src="https://user-images.githubusercontent.com/20410193/160065371-dc54fe0b-d4b6-4af3-bd5f-648187ba941c.png">


- **SSL**은 **암호화된 링크를 수립**하기 위한 표준 보안 기술

- **비대칭키**에서는 **암호화와 복호화**를 할 때 사용하는 **키가 서로 다르**기 때문에 메시지를 전송하는 쪽이 **공개키로 데이터를 암호화**하고, 수신 받는 쪽이 **비공개키로 데이터를 복호화**하면 되는 **이상적인 통신** 방법. 하지만 해당  방식의 암호화는 매우 **많은 컴퓨터 자원**을 사용.

  반면에 **대칭키 방식**(**암호화와 복호화**에 사용되는 **키가 동일**)은 **적은 컴퓨터 자원**으로 암호화를 수행할 수 있기 때문에 효율적이지만 수신측과 송신측이 **동일한 키를 공유**해야 하기 때문에 **보안의 문제**가 발생

  => 그래서 SSL은 **공개키와 대칭키의 장점을 혼합**한 방법을 사용

- 최종 사용자가 볼 수 없는 '**SSL Handshake**'라는 프로세스를 통해 웹 서버와 브라우저 간에 안전한 연결이 수립 됨

  <img width="551" alt="image" src="https://user-images.githubusercontent.com/20410193/160065389-7234a7d3-fced-4758-9193-6364078c2aa9.png">
  
  1. 서버에서는 **비대칭 공개 키**의 복사본을 브라우저로 전송

  2. 브라우저에서는 **대칭 세션 키**를 만들어 서버의 비대칭 **공개 키로 암호화**한 다음 이를 서버로 전송

  3. 서버는 **대칭 세션 키를 얻**기 위해 **비대칭 비공개 키를 사용**하여 **암호화된 세션 키를 해독**

  4. 이제 서버와 브라우저에서 **대칭 세션 키를 사용**하여 전송된 **모든 데이터를 암호화 및 해독**

     **브라우저와 서버에만 대칭 세션 키에 대한 정보**가 있으며 세션 키는 해당하는 **특정 세션에만 사용**되기 때문에 채널 보안이 보장됨. 다음날 브라우저가 동일한 서버에 다시 연결되면 새로운 세션 키가 생성



# 출처

- [HTTPS와 SSL 인증서](https://opentutorials.org/course/228/4894)
- [[OpenSSL] SSL/TLS 인증서 발급받기](https://heodolf.tistory.com/94)
- [iOS 13 및 macOS 10.15의 신뢰할 수 있는 인증서에 대한 요구 사항](https://support.apple.com/ko-kr/HT210176) 
- [[SSL]HTTPS통신을 위한 SSL인증서 발급하기(OpenSSL)](https://namjackson.tistory.com/24)
- [어떠한 요구 사항도 충족하는 SSL 인증서](https://www.digicert.com/kr/ssl-certificate/)
- [TLS(SSL) - 2. 인증서, CA, SSL 인증서를 통해 서버를 인증하는 방법](https://babbab2.tistory.com/5)
- [SSL인증서 심사 수준에 따른 레벨 구분방법 (DV, OV, EV)](https://cert.crosscert.com/%EF%BB%BFssl%EC%9D%B8%EC%A6%9D%EC%84%9C-%EC%8B%AC%EC%82%AC-%EC%88%98%EC%A4%80%EC%97%90-%EB%94%B0%EB%A5%B8-%EB%A0%88%EB%B2%A8-%EA%B5%AC%EB%B6%84%EB%B0%A9%EB%B2%95-dv-ov-ev/)
- [와일드카드 인증서](https://www.digicert.com/kr/wildcard-ssl-certificates/)
- [보안서버 인증서 종류가 많아서 선택하기 어려워요](https://www.koreassl.com/ssl)
- [SSL인증서 Wildcard VS SAN 비교, SSL가격 SSL발급 기준](https://m.blog.naver.com/cr0sscert/221870416199)

