

# 프로비저닝 프로파일(Provisioning profile)

## TL;DR

- **오직 Apple만이 앱 실행**을 허용

- 다만 **프로비저닝 프로파일**을 사용함으로써 개발자가 **Apple을 대신해 디버그 및 테스팅용 배포**를 할 수 있도록 허용

  즉, 프로비저닝 프로파일은 개발 및 테스트를 위해 iOS 장치에서 애플리케이션을 실행할 수 있도록 Apple에서 발급한 특별 hall pass

- 프로비저닝 프로파일은 앱 ID, 배포 인증서, 권한 목록 및 UDID 목록 등으로 구성 됨. 이 정보가 **앱 프로젝트 설정과 일대일 매핑** 되어야 함

-------

- 기기에서 **앱을 실행**하고 **특정 서비스를 사용**하고자 할 때 사용되는 **파일**

- 디바이스에서 앱을 실행하기 위해서는 내 디바이스가 **개발자를 신뢰**할 수 있는지를 알아야 **앱 설치**를 허락할지 말지를 결정할 수 있는데, 이 역할을 해주는 것이 Provisioning Profile

- 프로비저닝 프로파일은 **iOS 디바이스들을 Apple 인증서와 연결하는 역할**을 담당.

- 이 결과로 만들어진 `*.mobileprovision` 파일은 iOS 앱을 **컴파일하는 과정에서 사용되며** 앱을 테스트하려고 하는 **디바이스에 설치**가 되어야 함.

- [Xcode](https://developer.apple.com/xcode/)에서 자동 생성하거나 [Apple Developer Program](https://developer.apple.com/)에서 생성 가능

  [![img](https://camo.githubusercontent.com/db0a076feed8cbd02312dde075d65bf94c24521a19b2fedf5f5ac38d567a292b/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323431393244353035383542444530343036)](https://camo.githubusercontent.com/db0a076feed8cbd02312dde075d65bf94c24521a19b2fedf5f5ac38d567a292b/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323431393244353035383542444530343036)



## Apple에서 iOS 앱 실행을 허용하는 방법

- **탈옥하지 않는 한** App Store 외부에서 애플리케이션을 다운 받는 것은 불가. **오직 Apple만이 iOS 기기에서 애플리케이션 실행을 허용** 할 수 있음.
- 그렇다면 **개발자와 테스터는** 실제 장치에서 애플리케이션을 어떻게 설치, 실행 및 디버깅 함? -> **프로비저닝 프로파일**을 사용하여 Apple은 개발자가 **Apple을 대신하여 iOS 장치에서 앱을 실행할 수 있도록 허용**
- **프로비저닝 프로필**은 개발자 계정에서 다운로드되어 **앱 번들에 포함** 됨.
- iOS 기기는 설치된 Provisiong Profile 이 **앱의 서명에 사용**된 **Certificate를 포함**하는 지 확인하고(앱이 컴파일 된 후 프로비저닝 프로파일 안에 있는 **인증서의 개인 키로 서명**되는데, 인증서의 **개인 키**가 프로비저닝 프로파일에있는 인증서의 **공개 키와 일치**하는지 확인하는 것), 기기의 **UDID**와 **App ID**도 맞는 지 확인한 후에 **설치를 허용**

## 프로비저닝 프로파일 구성 목록

- **프로비저닝 프로파일**은 **개발 및 테스트 목적으로 iOS 장치에서 애플리케이션을 실행할 수 있도록 Apple에서 발급**하는 것
- **인증서**와 **기기 목록**, **`Entitlements` 항목**, **유효기간** 등 여러 항목으로 구성되어 있음. 
- 만약 실행 환경이 프로비저닝 프로파일에 명시된 자격 조건과 맞지 않는다면 앱 실행 불가.
- 프로비저닝 프로파일은 빌드된 앱(`.ipa` 파일) 내부에서 확인 가능
- `.ipa` 파일의 압축을 풀면 `Payload` 디렉터리 안에 `embedded.mobileprovision`란 이름의 파일(예: `Payload/sample.app/embedded.mobileprovision`)이 프로비저닝 프로파일.

대표적인 항목들은 아래와 같음

- **앱 ID**

- Apple에서 발급 한 **인증서** (certificate)

  - 이는 Apple이 **앱의 개발자 또는 게시자**로서 **나를 고유하게 식별하는 데 사용**하는 것

  - 인증서에는 **공개 / 개인 키 쌍**이 포함 됨

  - 배포 인증서의(distribution certificate) **개인 키는 애플리케이션에 서명** (signing) 하는 데 사용

  - 프로비저닝 프로파일 내 `DeveloperCertificates` 항목을 보면 [base64](https://developer.mozilla.org/en-US/docs/Web/API/WindowBase64/Base64_encoding_and_decoding)로 인코딩되어 있는 문자열을 `openssl` 명령어를 이용해 잘 확인해보면, 아래와 같이 파일로 저장된 인증서 확인 가능

    ```
    $ openssl x509 -in sample.txt -text
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number: XXXXXX... (XXXXXX... )
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: C = US, O = Apple Inc., OU = Apple Worldwide Developer Relations, CN = Apple Worldwide Developer Relations Certification Authority
            Validity
                Not Before: Oct 26 09:45:00 2018 GMT
                Not After : Oct 26 09:45:00 2019 GMT
            Subject: UID = XXXXXXXX, CN = iPhone Developer: XXXXXX (XXXXXXXX), OU = XXXXXXXX, O = XXXXXX, C = US
            Subject Public Key Info:
    .
    .
    ```

- 앱 실행이 허용 된 기기의 **UDID 목록**

  `ProvisionedDevices` 항목에서 확인 가능. 앱은 이 목록에 있는 기기에서만 실행 가능.

  ***AD hoc Provisioning Profile (기기 제한 있음)***

  ```
  <key>ProvisionedDevices</key>
  <array>
  <string>sampleABCDEFG</string>
  <string>sampleABCDEFGHIJK</string>
  </array>
  ```

  ***Inhouse Provisioning Profile (Enterprise로 빼낸거라 기기 제한 없음)***

  ```
  <key>ProvisionsAllDevices</key>
  <true/>
  ```

  ([원출처](https://engineering.linecorp.com/ko/blog/ios-code-signing/#Entitlements)에서는 'Ad Hoc 등 모든 기기에서 실행 가능한 앱의 프로비저닝 프로파일에는 아래와 같이 ProvisionsAllDevices 항목이 true로 설정되어 있습니다' 라고 써있는데, 'Ad Hoc 등 모든 기기에서 실행 가능'이라는 말에 의문이 들었다. 내가 아는 한 AdHoc이 100대 제한이고, Enterprise (InHouse)가 무제한이기 때문. 그래서 각각의 옵션으로 ipa 파일 빼낸 후 데이터를 확인해 봤는데 InHouse 의 `ProvisionsAllDevices`가 true 로 되어있었고, AdHoc은 등록된 기기의 목록이 있었다.)

- 푸시 알림, Passbook, HealthKit, CloudKit 등과 같이 앱에서 사용할 수있는 특별한 **Entitlements** (권한/자격)

  `Entitlements` 항목에서 확인 가능. **어떤 서비스를 사용할 수 있는지**에 대한 자격 증명이 명시되어 있음.

  위 항목 내 배열(`array`)은 Xcode 빌드 설정의 **Capabilities** 탭에서 무엇을 선택했느냐에 따라 달라짐. (Ex. **Access WiFi Information** 기능과 **Apple Pay** 기능)

- **유효 기간**

  `ExpirationDate` 항목에서 확인 가능. 이 항목에 명시된 유효기간이 지나면 앱 실행 불가.

## 모든 것이 통합된 프로비져닝 프로필

- 프로비저닝 프로파일은 **Apple 개발자 포털에서 생성** 됨
- 프로젝트 설정에서 **인증서와 프로비저닝 프로파일을 지정** 해야 함
- 이 작업은 Apple 개발자 포털에서 만든 다음 Mac에 다운로드 / 설치 한 후에 수행 됨. 가장 쉬운 방법은 XCode를 통하거나 수동으로 다운로드하여 키 체인에 설치 가능.

[![모두 함께 묶기](https://camo.githubusercontent.com/ed3964b0b220fed27958bbf80d4df5a7608a0e354b82f71de78645f3bd6c9e1b/687474703a2f2f73686172706d6f62696c65636f64652e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031352f30312f4170702d546f2d50726f66696c652d4d617070696e67312d31303234783730312e706e67)](https://camo.githubusercontent.com/ed3964b0b220fed27958bbf80d4df5a7608a0e354b82f71de78645f3bd6c9e1b/687474703a2f2f73686172706d6f62696c65636f64652e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031352f30312f4170702d546f2d50726f66696c652d4d617070696e67312d31303234783730312e706e67)

- 이미지에서 볼 수 있듯, 디버깅 및 테스트를 위해 앱을 iOS 기기에 설치하기 전에 특정 항목을 일치 시켜야 한다.
  - 번들 ID가 프로비저닝 프로필의 앱 ID와 일치
  - 프로젝트 설정의 권한 목록은 프로비저닝 프로파일에 지정된 권한 목록과 일치 (Entitlements가 필요하지 않으면 무시됨.)
  - 장치의 UDID가 프로비저닝 프로파일의 장치 UDID 목록에 포함
  - 앱이 컴파일 된 후 배포 인증서의 개인 키로 서명되는데, **인증서의 개인 키가 프로비저닝 프로파일에있는 인증서의 공개 키와 일치** (내가 이 앱을 컴파일했고 나를 가장 한 사람이 아니라는 것을 Apple에 알려주는 역할)

## 프로비저닝 프로파일 유형

- 목적에 따라 다른 유형의 프로비저닝 프로파일을 사용 (Development, Ad-Hoc, App Store, Enterprise)

- Development

   

  provisioning profiles

  - 단일 기기에서 앱을 테스트하기 위한 것. 즉, **실제 디바이스**를 연결 후 **Xcode 돌려서** 테스트하고 싶을 때 사용
  - **Development** Signing **Certificates** 사용

- Distribution

   

  provisioning profiles

  - 베타 테스터나 앱 스토어에 앱을 제출하려는 경우에 사용. 즉, **Xcode 외의 장소**에서 애플리케이션을 배포하는 경우
  - **App Store / Ad Hoc / Enterprise (in house)** 배포가 포함됨
  - **Distribution Certificates** 사용 [![image](https://user-images.githubusercontent.com/20410193/137158488-6e26fc8e-9ff6-46af-8bed-572e2ccb73bc.png)](https://user-images.githubusercontent.com/20410193/137158488-6e26fc8e-9ff6-46af-8bed-572e2ccb73bc.png)

### 1. Development

- 개별 개발자에게 할당되며 이들을 식별하는 데 사용 됨
- 개발 및 디버그 빌드 시에 사용
- 개발 프로비저닝 프로파일은 Development Signing Certificates로만 생성 가능
- 자세한 내용은 애플의 [distribution provisioning profile](https://help.apple.com/xcode/mac/current/#/deveca502cb1)  문서 참고

### 2. Ad-Hoc

- Development provisioning profile과 유사하지만 QA 테스터에게 배포하는 데 사용

- 특정 iOS 테스트 장치의 소규모 그룹에서 응용 프로그램을 테스트하는 실용적인 방법

- 푸시 알림과 같은 다양한 권한 부여 서비스를 테스트하려는 경우에는 개발 프로파일로는 테스트 불가

- Ad-Hoc provisioning profile은 Production (Ad-Hoc) Certificates로만 생성 가능

- 자세한 내용은 애플의 [ad hoc provisioning profile](https://help.apple.com/xcode/mac/current/#/dev4335bfd3d) 문서 참고

  ![img](https://help.apple.com/xcode/mac/current/en.lproj/Art/ad_hoc_provisioning_launch_2x.png)

### 3. Enterprise

- Ad-Hoc provisioning profile과 유사하지만 몇 가지 중요한 차이점 존재
- 회사가 [Apple iOS 개발자 엔터프라이즈 프로그램](https://developer.apple.com/programs/ios/enterprise/) ($ 299 USD / 년)에 가입해야 얻을 수 있음
- UDID 목록이 필요하지 않음. 모든 장치에서 앱을 실행 가능. 따라서 기업은 장치 수에 제한없이 사내 앱을 유연하게 설치할 수 있음.
- 이때 개발자에게 개발자가 정말로 애플인것처럼 개발한 앱들을 서명할 수 있는 인증서를 발급해주기 때문에 이 인증서로 서명된 앱들은 따로 확인을 거치지 않고, 모든 디바이스를 애플에 등록하지 않고도 개발된 앱을 디바이스에서 바로 실행할 수 있도록 해줌

### 4. App Store

- Apple App Store에 애플리케이션을 제출할 때 사용하려는 프로파일
- 앱이 QA를 통과하고 완전히 테스트 된 후에 사용
- 빌드된 앱이 어떤 디바이스에서도 실행이 되지 않도록 함. 즉, 앱스토어 제출 용도 말고는 어디에서도 쓸 수가 없음
- Apple은 앱이 App Store 프로비저닝 프로파일로 빌드되지 않은 경우 앱 제출을 허용하지 않음
- Apple이 앱을 App Store에 배포하도록 승인하면 자체 서명 인증서 및 프로필을 사용하여 애플리케이션에 다시 서명 함. 따라서 모든 iOS 디바이스에서 실행될 수 있도록 해줌
- 자세한 내용은 애플의 [distribution provisioning profile](https://help.apple.com/xcode/mac/current/#/deveca502cb1) 문서 참고

### 5. 기타 

- Copy App, Developer ID 메소드도 있긴 한데 이건 macOS 에 해당. 자세한건 [Distribution methods](https://help.apple.com/xcode/mac/current/#/dev31de635e5) 을 참고.


## 프로비저닝 프로파일의 한계

- 서명 인증서가 만료되면 프로비저닝 프로파일도 만료

# 출처

- [Making Sense Of iOS Provisioning](https://www.sharpmobilecode.com/making-sense-of-ios-provisioning/)
- [인증서와 코드 사이닝(signing:서명)과 배포](https://doorganizedcoding.tistory.com/4)
- [[iOS\] 인증서와 코드 사이닝 이해하기](http://la-stranger.blogspot.com/2014/04/ios.html)
- [[iOS\] Code Signing & Provisioning Profile 이해하기](https://m.blog.naver.com/mym0404/221611576550)
- [iOS Code Signing & Provisioning in a Nutshell](https://medium.com/ios-os-x-development/ios-code-signing-provisioning-in-a-nutshell-d5b247760bef)
- [What is a provisioning profile & code signing in iOS?](https://abhimuralidharan.medium.com/what-is-a-provisioning-profile-in-ios-77987a7c54c2)
- [iOS code signing](https://docs.codemagic.io/flutter-code-signing/ios-code-signing/)
- [ProvisioningProfileType](https://docs.unity3d.com/ScriptReference/ProvisioningProfileType.html)
- [iOS Code Signing, Development and Distribution Provisioning Profiles explained](https://getupdraft.com/blog/ios-code-signing-development-and-distribution-prov)
- [iOS Code Signing breakdown](https://bruno-lorenzop.medium.com/ios-code-signing-breakdown-766d95c89f20)
- [iOS 코드 서명에 대해서](https://engineering.linecorp.com/ko/blog/ios-code-signing/#Entitlements)
- [[iOS\] 코드 사이닝 (프로비저닝 프로파일, 인증서)](https://beankhan.tistory.com/115)
- [Distribution methods](https://help.apple.com/xcode/mac/current/#/dev31de635e5)
