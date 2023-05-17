# Firebase Dynamic Link

- 앱 설치 여부에 관계없이 여러 플랫폼에서 원하는 대로 작동하는 링크
- 단일 링크를 통해 앱이 설치된 경우 앱으로 바로 이동할 수 있고, 그렇지 않은 경우 해당 앱 스토어로 이동 가능

  ```
  https://your_subdomain.page.link/?link=your_deep_link&apn=package_name[&amv=minimum_version][&afl=fallback_link]
  ```

  ![동적 링크와 iOS 간의 통합을 자세히 설명하는 플로 차트](https://firebase.google.com/docs/dynamic-links/images/fdl-ios-integration.png?hl=ko)

## Dynamic Link는 어떻게 iOS 앱으로 전달되는가

- 두가지 방법 존재 

### 1. universal links

- 웹 사이트에 `apple app site association` file 을 배치하여 웹 사이트를 iOS 앱에 매핑하는 방법

- 해당 파일은 사이트의 특정 위치에 있는 모든 콘텐츠를 앱에서도 사용할 수 있음을 iOS에 알려주므로, iOS가 이러한 URL을 발견하면 Safari로 이동하지 않고 앱이 열린다. 그런 다음 해당 URL을 앱으로 전달하여 앱에서 해당 URL을 처리하고 사용자에게 올바른 정보를 표시할 수 있다.

  ![image](https://user-images.githubusercontent.com/20410193/116708121-5d800d00-aa0a-11eb-8e3b-7fb6609fb9e6.png)

- 내 웹사이트에 universal link 설정해본적 없는데..??하고 당황하지 말자. 

- Firebase Dynamic Links를 사용하면, 모든 링크들은 `https://~~.page.link` 와 비슷한 형식의 도메인으로 설정되는데, 파이어베이스는 그 도메인에 작은 미니 웹사이트를 만든다.

- 그런 다음 이 도메인에서 앱으로 바로 연결되는 자체 Apple App Site 연결 파일을 추가한다. 따라서 iOS가 이 도메인의 URL로 실행되면 대신 앱이 열린다.

- 따라서 동적 링크를 위한 웹 사이트를 설정을 할 필요가 없다. 파이어베이스가 알아서 대부분의 작업을 수행해주기 때문.
  ![image](https://user-images.githubusercontent.com/20410193/116708169-65d84800-aa0a-11eb-8c70-b3edeb9ed4ba.png)


### 2. Custom URL Schemes

- Dynamic Link를 만드는 또 다른 방법

- 기본적으로 이러한 URL을 일반적인 URL처럼 생각할 수 있다. 단, https로 시작하는 대신 앱과 관련된 고유한 것으로 시작한다.

  ![image](https://user-images.githubusercontent.com/20410193/116708235-75f02780-aa0a-11eb-8f3c-6c305e3dc7b2.png)

- 기본적으로 앱은 iOS에게 이 custom scheme으로 시작하는 URL을 요청(claim)할 것이라고 알려준다. 이후 iOS가 이 중 하나를 만나면, 앱을 열기위해 해당 URL을 앱에 전달한다.

- Firebase Dynamic Links는 default 로 앱의 번들 ID를 URL shceme으로 사용한다. 만약 앱이 이미 custom URL scheme을 사용하고 있다면 이 값을 변경할 수 있다. 하지만 그렇지 않으면, default를 그대로 사용하는게 훨씬 편리하다.

  ![image](https://user-images.githubusercontent.com/20410193/116708261-7c7e9f00-aa0a-11eb-9289-99d024530f3d.png)


따라서 앱이 universal link와 custom url scheme을 모두 지원하는지 확인해야 한다.

## iOS 기반 Firebase에서 Dynamic Links를 구현하는 방법 (3 Steps)

### 1. Firebase 콘솔과 Xcode 프로젝트에서 몇 가지 추가 설정

#### Universal Link를 위한 설정

**In Firebase**

- 앱스토어 ID 설정

- App ID prefix 설정 (대부분 팀 ID와 동일)

- 동적 링크에 사용할 도메인 설정 (ex. `myCoolGame`)

  - 이때 다른 Firebase 프로젝트에서는 이 sub domain을 사용할 수 없다. 따라서 프로젝트에 특정한 것으로 만들어야한다.  (회사 이름이 아니라, 앱 이름 같은 것으로)

    ![image](https://user-images.githubusercontent.com/20410193/116708289-856f7080-aa0a-11eb-9b70-eb05ea7bf390.png)


  - 또한 여기에 입력하는 내용에 따라 웹 사이트의 DNS entry에 해당 브랜드의 실제 owner임을 확인하는 인증 text를 추가해야할 수 있다. 따라서 `google.page.link` 같은 것은 사용할 수 없음 

  - 이제 Firebase는 우리의 앱을 가리키는 `apple app site association` 파일이 포함된 작은 웹사이트를 만든다. (`https://~~.page.link/apple-app-site association` ). 실제로 나머지 프로세스를 진행하기 전에 이 사이트를 확인하고 실행하는 것이 좋다.

    ```
    {
      "applinks": {
        "apps": [
          
        ],
        "details": [
          {
            "appID": "~~~~",
            "paths": [
              "NOT /_/*",
              "/*"
            ]
          }
        ]
      }
    }
    ```

  - 해당 파일이 의미하는 것은 도메인(`https://~~.page.link`)으로 전송된 모든 URL (paths) 이, 번들 ID(appID)를 가진 앱으로 직접 이동한다 라는 뜻.

**In Xcode**

- target -> Capabilities -> associated domains 활성화 (만약 활성이 안되면  권한 가진 사람이 Apple Developer Portal에서 따로 사용 설정 필요)
- `applinks:~~.page.link` 추가를 통해 동적 링크의 특정 도메인 활성화

#### Custom URL Schemes을 위한 설정

**In Xcode**

- target -> info -> URL Types 추가
  -  Identifier : 비워둬도 되지만 "Bundle ID" 등으로 설정해주는게 좋다
  -  URL Schems : `com.example.kerp.RecipyRally` 같은 실제 bundle id 로 설정. Dynamic Link가 defualt로 사용하는 custom URL scheme

#### 테스트

- 유니버설 링크는 safari 에 복붙해도 작동 안하고, 시뮬레이터에서도 작동하지 않는다.
- 실제 기기의 Notes나 iMessage에 있는 URL을 클릭해야 한다.

### 2. 실제 동적 링크 생성

- 대개의 동적 링크는 클라이언트 라이브러리나 REST API를 사용하여 만들어질 것 
- 하지만 소셜 캠페인에 포함시키거나 테스트 목적으로만 수동으로 동적 링크를 생성하려는 경우 Firebase Console에서 이 작업을 수행할 수 있다
  - 파이어베이스 콘솔에서 Dynamic Links section 클릭 -> New Dynamic Link 버튼 클릭 
  - 딥 링크 URL 파라미터는 iOS나 Android 앱으로 전달될 데이터. 
  - 링크를 iOS에서 열 때 어떤 일이 일어나야 하는지 지정 -> 앱 열기
  - 앱이 설치되지 않은 경우 수행할 작업을 선택 -> 앱스토어 열기
  - 링크를 Android에서 열 때 어떤 일이 일어나야 하는지 지정 -> 미설정
  - 고급 옵션 설정

### 3. Dynamic Links를 읽어 들이고 구문을 분석할 수 있도록 몇 가지 코드를 Appdelegate에 추가

- 앱에서는 universal link 또는 custom url scheme 을 통해 동적 링크를 URL로 수신한다. 그러면 앱이 이러한 URL을 DynamicLinks 라이브러리로 전달하여 Firebase dynamicLink 개체로 변환한다. (URL 적절한 동적 링크가 아닌 경우 null 개체로 변환) 그런 다음, 실제로 관심 있는 데이터를 포함하는 원래의 딥 링크 URL 매개 변수를 추출할 수 있다. 

  ![image](https://user-images.githubusercontent.com/20410193/116708331-8ef8d880-aa0a-11eb-8454-334610e53fab.png)

- Firebase/DynamicLinks를 pod 에 추가하고 install 

- 이제 받은 URL은 두 가지 방법으로 앱에 연결할 수 있으며 두 가지 다른 방법으로 처리된다. 

#### 1. Universal Link 

- AppDelegate의 `application(_:continue:restorationHandler:)` 함수를 사용한다.
- 위 함수에서 인자로 받은 `userActivity.webpageURL` 을 받아서 Firebase dynamicLink  처리를 한다.

#### 2. Custom URL Scheme

- 최신 버전의 iOS를 실행하는 경우에도 일반적으로 사용자가 처음 앱을 설치할 때  Custom URL Scheme 통해 전송되는 dynamic link 처리가 필요하다.

- AppDelegate의 `application(_:open:options:)` 함수를 사용한다.

- 위 함수에서 인자로 받은 url을 로 받아서 Firebase dynamicLink  처리를 한다.

## 동적 링크 URL 이 만들어지는 방식

- Firebase dynamic link 는 앱이 설치 유무 등 상황에 따라 호출되는 포맷이 다른데 다음 형식으로 **URL을 직접 작성**하여 **동적 링크**를 만들 수 있다. (출처 - [동적 링크 URL 직접 만들기](https://firebase.google.com/docs/dynamic-links?hl=ko) )

| 매개변수 |                                                              |
| :------- | :----------------------------------------------------------- |
|          |                                                              |
| efr      | '1'로 설정하는 경우, 동적 링크가 열리면 앱 미리보기 페이지를 건너뛰고 앱 또는 스토어로 리디렉션합니다. 기본적으로 사용 설정되는 앱 미리보기 페이지는 앱에서 동적 링크를 연 사용자를 가장 적절한 목적지로 더 안정적으로 안내할 수 있지만, 이 페이지 없이 동적 링크를 안정적으로 열 수 있는 앱에서만 동적 링크가 열리게 하려면 이 매개변수를 사용하여 리디렉션을 중지할 수 있습니다. 이 매개변수는 iOS에서만 동적 링크의 동작에 영향을 미칩니다. |
| ipbi     | iPad에서 링크를 여는 데 사용할 iOS 앱의 번들 ID입니다. Firebase Console의 개요 페이지에서 앱을 프로젝트에 연결해야 합니다. |
| ibi      | 링크를 여는 데 사용할 iOS 앱의 번들 ID입니다. Firebase Console의 개요 페이지에서 앱을 프로젝트에 연결해야 합니다. 동적 링크를 통해 iOS 앱을 여는 데 필요합니다. |
| imv      | 링크를 열 수 있는 앱의 최소 [버전 번호](https://developer.apple.com/library/content/technotes/tn2420/_index.html)입니다. 이 플래그는 앱이 열렸을 때 앱에 전달되며 앱에서 수행할 작업을 결정해야 합니다. |
| link     | 앱에서 열리는 링크입니다. 앱이 처리할 수 있는 URL(일반적으로 앱의 콘텐츠 또는 페이로드)을 지정할 수 있으며, 이를 통해 사용자에게 쿠폰을 지급하거나 시작 화면을 표시하는 등 앱의 특정 로직을 시작할 수 있습니다. 이 링크는 올바른 형식의 URL이어야 하며, 적절한 URL 인코딩을 적용해야 합니다. HTTP 또는 HTTPS를 사용해야 하며 다른 동적 링크가 아니어야 합니다.(`ofl` 매개변수가 지정되지 않은 상태에서) 사용자가 데스크톱 웹브라우저에서 동적 링크를 열면 이 URL이 로드됩니다. 연결된 콘텐츠에 해당하는 웹 콘텐츠가 없다면 URL이 유효한 웹 리소스를 반드시 가리킬 필요는 없습니다. 이러한 경우 이 URL에서 홈페이지 등으로 연결되도록 리디렉션을 설정해야 합니다. |
| ius      | 앱의 번들 ID와 다른 값으로 정의하면 앱의 커스텀 URL 스킴으로 사용됩니다. |
| ipfl     | 앱이 설치되지 않은 경우 iPad에서 열리는 링크입니다. 앱이 설치되지 않았을 때 App Store에서 앱을 설치하는 것 이외에 콘텐츠의 웹 버전 열기, 앱 홍보 페이지 표시 등의 다른 작업을 수행하려는 경우 이 매개변수를 지정합니다. |
| isi      | 앱이 설치되지 않은 경우 사용자를 App Store로 보내는 데 사용되는 앱의 App Store ID입니다. |
| ifl      | 앱이 설치되지 않은 경우에 열리는 링크입니다. 앱이 설치되지 않았을 때 App Store에서 앱을 설치하는 것 이외에 콘텐츠의 모바일 웹 버전 열기, 앱 홍보 페이지 표시 등의 다른 작업을 수행하려는 경우 이 매개변수를 지정합니다. |
  

# 출처

- [Firebase 동적 링크](https://firebase.google.com/docs/dynamic-links?hl=ko)
- https://firebase.google.com/docs/dynamic-links/ios/receive
- [운영체제 통합](https://firebase.google.com/docs/dynamic-links/operating-system-integrations?hl=ko)
- [Getting started with Dynamic Links on iOS - Pt.2 (Firecasts)](https://www.youtube.com/watch?v=iSC5ed6OowA)

