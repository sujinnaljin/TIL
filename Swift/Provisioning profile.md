# 프로비저닝 프로파일(Provisioning profile)

## TL;DR

- 오직 Apple만이 앱 실행을 허용

- 다만 프로비저닝 프로파일을 사용함으로써 개발자가 Apple을 대신해 디버그 및 테스팅용 배포를 할 수 있도록 허용

  즉, 프로비저닝 프로파일은 개발 및 테스트를 위해 iOS 장치에서 애플리케이션을 실행할 수 있도록 Apple에서 발급한 특별 hall pass

- 프로비저닝 프로파일은 앱 ID, 배포 인증서, 권한 목록 및 UDID 목록 등으로 구성

- 프로비저닝 프로파일로 서명 된 앱은, 앱 프로젝트 설정과 프로비저닝 프로필이 일대일 매핑 되어야 함

- 목적에 따라 다른 유형의 프로비저닝 프로파일을 사용 (Development, Ad-Hoc, App Store, Enterprise)


## Apple에서 iOS 앱 실행을 허용하는 방법

- **탈옥하지 않는 한**  App Store 외부에서 애플리케이션을 다운 받는 것은 불가. **오직 Apple만이 iOS 기기에서 애플리케이션 실행을 허용** 할 수 있음.
- 그렇다면 **개발자와 테스터는** 실제 장치에서 애플리케이션을 어떻게 설치, 실행 및 디버깅 함? -> **프로비저닝 프로파일**을 사용하여 Apple은 개발자가 **Apple을 대신하여 iOS 장치에서 앱을 실행할 수 있도록 허용** 
- iOS 기기는 설치된 Provisiong Profile 이 **앱의 서명에 사용**된 **Certificate를 포함**하는 지 확인하고, 기기의 **UDID**와 **App ID**도 맞는 지 확인한 후에 **설치를 허용**

## 프로비저닝 프로파일 구성 목록

**프로비저닝 프로파일**은 **개발 및 테스트 목적으로 iOS 장치에서 애플리케이션을 실행할 수 있도록 Apple에서 발급**하는 것으로, 여러 항목으로 구성되어 있음. 대표적인 항목들은 아래와 같다.

- **앱 ID**  
- Apple에서 발급 한 **인증서** (certificate)

  - 이는 Apple이 **앱의 개발자 또는 게시자**로서 **나를 고유하게 식별하는 데 사용**하는 것
  - 인증서에는 **공개 / 개인 키 쌍**이 포함 됨
  - 배포 인증서의(distribution certificate) **개인 키는 애플리케이션에 서명** (signing) 하는 데 사용
- 앱 실행이 허용 된 기기의 **UDID 목록**
- 푸시 알림, Passbook, HealthKit, CloudKit 등과 같이 앱에서 사용할 수있는 특별한 **Entitlements** (권한/자격)

## 모든 것이 통합된 프로비져닝 프로필

- 프로비저닝 프로파일은 **Apple 개발자 포털에서 생성** 됨
- 프로젝트 **설정에서 인증서와 프로비저닝 프로파일을 지정** 해야 함
- 이 작업은 Apple 개발자 포털에서 만든 다음 Mac에 다운로드 / 설치 한 후에 수행 됨. 가장 쉬운 방법은 XCode를 통하거나 수동으로 다운로드하여 키 체인에 설치 가능.

![모두 함께 묶기](http://sharpmobilecode.com/wp-content/uploads/2015/01/App-To-Profile-Mapping1-1024x701.png)

- 이미지에서 볼 수 있듯, 디버깅 및 테스트를 위해 앱을 iOS 기기에 설치하기 전에 특정 항목을 일치 시켜야 한다.
  - 번들 ID가 프로비저닝 프로필의 앱 ID와 일치
  - 프로젝트 설정의 권한 목록은 프로비저닝 프로파일에 지정된 권한 목록과 일치 (Entitlements가 필요하지 않으면 무시됨.)
  - 장치의 UDID가 프로비저닝 프로파일의 장치 UDID 목록에 포함
  - 앱이 컴파일 된 후 배포 인증서의 개인 키로 서명되는데, **인증서의 개인 키가 프로비저닝 프로파일에있는 인증서의 공개 키와 일치** (내가 이 앱을 컴파일했고 나를 가장 한 사람이 아니라는 것을 Apple에 알려주는 역할)

## 프로비저닝 프로파일 유형

### 1. Development

- 개별 개발자에게 할당되며 이들을 식별하는 데 사용 됨
- 개발 및 디버그 빌드 시에 사용
- 개발 프로비저닝 프로파일은 Development Signing Certificates로만 생성 가능

### 2. Ad-Hoc

- Development provisioning profile과 유사하지만 QA 테스터에게 배포하는 데 사용
- 푸시 알림과 같은 다양한 권한 부여 서비스를 테스트하려는 경우에는 개발 프로파일로는 테스트 불가
- Ad-Hoc provisioning profile은 Production (Ad-Hoc) Certificates로만 생성 가능

### 3. Enterprise 

- Ad-Hoc provisioning profile과 유사하지만 몇 가지 중요한 차이점 존재
- 회사가 [Apple iOS 개발자 엔터프라이즈 프로그램](https://developer.apple.com/programs/ios/enterprise/) ($ 299 USD / 년)에 가입해야 얻을 수 있음
- UDID 목록이 필요하지 않음. 따라서 기업은 장치 수에 제한없이 사내 앱을 유연하게 설치할 수 있음.

### 4. App Store

- Apple App Store에 애플리케이션을 제출할 때 사용하려는 프로파일
- 앱이 QA를 통과하고 완전히 테스트 된 후에 사용
- 빌드된 앱이 어떤 디바이스에서도 실행이 되지 않도록 함. 즉, 앱스토어 제출 용도 말고는 어디에서도 쓸 수가 없음
- Apple은 앱이 App Store 프로비저닝 프로파일로 빌드되지 않은 경우 앱 제출을 허용하지 않음
- Apple이 앱을 App Store에 배포하도록 승인하면 자체 서명 인증서 및 프로필을 사용하여 애플리케이션에 다시 서명 함. 따라서 모든 iOS 디바이스에서 실행될 수 있도록 해줌

## 프로비저닝 프로파일의 한계

- 서명 인증서가 만료되면 프로비저닝 프로파일도 만료

# 출처

- [Making Sense Of iOS Provisioning](https://www.sharpmobilecode.com/making-sense-of-ios-provisioning/)
- [인증서와 코드 사이닝(signing:서명)과 배포](https://doorganizedcoding.tistory.com/4)
- [[iOS] 인증서와 코드 사이닝 이해하기](http://la-stranger.blogspot.com/2014/04/ios.html)
- [[iOS] Code Signing & Provisioning Profile 이해하기](https://m.blog.naver.com/mym0404/221611576550)
- [iOS Code Signing & Provisioning in a Nutshell](https://medium.com/ios-os-x-development/ios-code-signing-provisioning-in-a-nutshell-d5b247760bef)


