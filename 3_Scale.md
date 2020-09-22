# 3 Scale 

## 1. API Economy

- API는 쉽게 얘기하자면 두 개의 소프트웨어가 서로 대화하고 데이터를 주고받는 규격이다. 
- API를 통해 **MSA(Micro Service Architecture)에서 쪼개진 서비스 간 연계**를 하기도 한다.
- 이러한 **API를 활용하기 위해 여러 API Management Platform**들이 시장에 나와있다. 
  - **Red Hat 3scale**
  - Google Apigee
  - Mulsoft API Manager
  - CA API Management

## 2. Red Hat 3scale API Management Platform

- API Management Platform은 말그대로 API를 잘 활용할 수 있도록 **접근 제어, 접근성 및 가시성(API 문서화), 모니터링, 과금** 등의 기능들을 하나의 플랫폼 형태로 만들어놓은 것이다.

- Red Hat의 3scale은 API Management Platform의 여러 요소들을 크게 **3가지 기능으로 구분하여 제공**하고 있다

  1. **Visibility (가시성)**

     API 공급자가 만들고 출시한 API에 대한 가시성을 제공하는 기능이다. 쉽게 말해서 내가 만든 **API 에 대한 정보 및 사용법 등을 제공**하는 포탈과 **API가 얼마나, 어떻게 활용되고 있는지**를 보기 위한 기능이다.

  2. **Control (관리)**

     API 호출에 대한 관리 기능이다. API 엔드 포인트에 **액세스 할 수 있는 사용자** 및 사용자가 **수행할 수 있는 작업**을 정의할 수 있는 기능이다.

  3. **Flexibility/Scalability (유연성/확장성)**

     요구사항이 변경될 때, **변경 사항에 빠르게 대응**하기 위한 **유연성**과, **사용량이 많아질 수록 빠르게 확장** 할 수 있는 **확장성** 기능이다.



## 3. 3scale Architecture

![](https://miro.medium.com/max/1400/1*ehVo0DsAU9br64YCHYEc4A.png)

- 3scale은 **Cloud에서 호스팅되는 hosted 버전**과, **직접 설치/구성하여 사용하는 Self-managed** 버전이 있다.

- Self-managed 구성은 아래와 같다.

  1. **APIcast API Gateway**

     3scale은 APIcast라는 NGINX기반의 API Gateway를 사용한다. 이 Gateway는 **API 관리 정책 실행 처리 (트래픽 관리)**를 수행한다.

     **Docker Image로 제공**되기 때문에 Openshift와 같은 **도커 컨테이너 환경에서 어디든 배포 가능**하다.

  2. **API Manager**

     API **관리 정책 구성, 분석, 청구**하는 기능을 제공한다. 사용자는 Admin Portal을 통해서 UI로 정책 등을 정의 가능하며, 이 API Manager에서 정의한 정책을 실제로 API Gateway가 실행한다.

     이 API manager도 **도커 이미지화** 되어 있으며 현재는 Openshift Container Platform 위에서만 구동 가능하다.

  3. **Developer Portal**

     공급자가 **소비자(개발자)에게 API를 노출하고 제공 할 수 있게 하는 포탈**이다. 개발자는 **이 포탈을 통해 API를 확인하고 사용**할 수 있다. 물론 회원 가입 등을 통해 **가입된 개발자만 활용**하도록 구성할 수 있다.

     **컨텐츠 관리 시스템** (CMS)과 Interactive **API 문서(ActiveDocs, Swagger 등)를 제공**하며, 표준 웹 기술을 기반으로 하므로 쉽게 자신의 회사에 맞게 커스터마이징이 가능하다.

## 4. Key Features

3scale의 Key Features는 5가지로 설명할 수 있다

1. **접근제어 및 보안 (Access control and security)**

   **백엔드 서비스를 보호하기 위해 API 액세스를 인증하고 제**한하는 기능이다.여러가지 다중 인증 메커니즘을 제공한다. (단순하게는 API 키, 애플리케이션 ID / 애플리케이션 키 쌍, 복잡하게는 OAuth 2.0)

   뿐만 아니라, 다양한 Identitiy 제공자(SSO, LDAP)와 통합 가능(ex) Stormpath, Red Hat SSO)할 뿐 아니라 소셜 로그인, ID federation을 지원한다.

   트래픽을 인증, 정책으로 제한하거나 트래픽이 비정상적으로 증가 시 초과 경보를 생성할 수 있다.

2. **API 계약 및 사용률 제한 (API contracts and rate limits)**

   Package 단위로 **API 엔드 포인트 액세스, 사용량, 과금 정책을 만들어 손쉽게 관리** 가능하다.

3. **분석 및 리포팅 (Analytics and reporting)**

   어떤 애플리케이션이나 개발자가 **어느 API 엔드 포인트에 액세스 하고 있는지 분석할 수 있도록 가시성**을 제공한다.

4. **개발자 및 파트너 포탈 (Developer and Partner Portal)**

   개발자에게 **API를 쉽게 이해시키고 사용**할 수 있도록 뛰어난 개발자 경험 (Developer Expereince)을 제공하는 **커스터마이징 가능한 포탈을 제공**한다.

   개발자 포탈은 다음과 같은 기능들을 제공한다.

   예 : 계정 / 애플리케이션 관리, 개발자 분석, API 키 관리, 온 보딩, API 문서

   또한 API 명세에 주로 사용되는 **Swagger 같은 개방형 산업 표준에 기반한 문서 OAI (Open Archives Initiative) 를 지원**한다.

5. **API 청구 및 과금 (API Billing and Payments)**

   API 소비자와 공급자간에 손쉬운 **엔드 투 엔드 (end-to-end) 청구**를 구현할 수 있도록 기능을 제공한다. 뿐만 아니라 Stripe, Braintree / PayPal, Adyen 등 결제 솔루션과 통합할 수 있도록 지원한다.

결국 **시스템 흐름도**를 정리해보면 다음과 같다.

1. API 공급자는 자신의 회사 내에서 만든 **API를 Developer Portal에 등록하고 이에 사용법/명세 등을 등록**한다. 
2. API Manager에서 제공하는 Admin portal를 통해 사용량이나 **인증, 과금 등의 정책**을 정한다. 이 정책은 API Gateway에 반영된다. 
3. API 사용자 (개발자)는 인증 후 개발자 포탈에 들어가서 API 활용법을 익히고, 테스트도 하고 자신의 App 내에 **API를 호출 하는 등 연계**한다.
4. 인증된 Developer App이 **API Gateway를 통해 API를 호출**하게 되며, API Gateway는 이 호출이 **정상적이고 인가된 호출인지 확인** 후 Back-End 시스템에 전달 후 **API 응답**을 보내게 된다. 
5. API Gateway에 들어오는 트래픽 유입, 활용 사용자, App, 사용률 등은 모두 API Manager로 보내서 **관리자가 분석**할 수 있도록 제공한다.

![Image for post](https://miro.medium.com/max/4080/1*yVtE1pkHgzP68HR-Su9aYw.png)



**출처** [[3scale API Management Platform] Overview](https://medium.com/@rlackdejr89/3-scale-api-management-platform-overview-187c861f798c)


