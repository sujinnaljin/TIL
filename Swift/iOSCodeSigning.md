# iOS 코드 서명 (feat. 프로비저닝 프로파일과 Mach-O 파일)

## 코드 서명

- 코드 서명이란 인증서를 이용해서 코드에 서명을 하는 것.

  - 자세히는 Mach-O 파일 안의 code signature > **code directory** 를 **인증서로 서명**하면 **signature data**가 나옴 (CMS 서명). code directory에는 특정 파일과 실행 바이너리 파일 조각들의 **해시값**들이 담겨져 있음.

  ![img](https://engineering.linecorp.com/wp-content/uploads/2019/04/26-1.png)

- 이 과정을 통해 **파일의 무결성을 검증**하고 **서명자(개발자)를 확인** 가능

- Apple의 앱은 **코드 서명**을 해야 iOS 기기에서 **실행 가능**

- **코드 서명**은 반드시 **Apple에서 발급**한 **인증서**로 진행되어야 함.

  - **앱스토어**에서 설치한 앱은 **Apple의 인증서**로 코드 서명
  - **개발자**가 테스트하거나 배포하는 앱은 Apple이 발급한 **개발자 인증서**로 코드 서명

- 이때 **개발자 인증서**로 코드 서명한 앱을 기기에 설치할 때는 **프로비저닝 프로파일**(provisioning profile) 필요.

  프로비저닝 프로파일에 명시된 기기에 프로비저닝 프로파일을 설치해야 Apple의 인증서로 코드 서명된 앱이 아니더라도 기기에서 실행 가능.

## 프로비저닝 프로파일

- 기기에서 **앱을 실행**하고 **특정 서비스를 사용**하고자 할 때 사용되는 **파일**

- 디바이스에서 앱을 실행하기 위해서는 내 디바이스가 **개발자를 신뢰**할 수 있는지를 알아야 **앱 설치**를 허락할지 말지를 결정할 수 있는데, 이 역할을 해주는 것이 Provisioning Profile

- 프로비저닝 프로파일은 **iOS 디바이스들을 Apple 인증서와 연결하는 역할**을 담당. 

- 이 결과로 만들어진 `*.mobileprovision` 파일은 iOS 앱을 **컴파일하는 과정에서 사용되며** 앱을 테스트하려고 하는 **디바이스에 설치**가 되어야 함.

- [Xcode](https://developer.apple.com/xcode/)에서 자동 생성하거나 [Apple Developer Program](https://developer.apple.com/)에서 생성 가능

  ![img](https://t1.daumcdn.net/cfile/tistory/24192D50585BDE0406)

### 프로비저닝 프로파일의 내용

- **인증서**와 **기기 목록**, **`Entitlements` 항목**, **유효기간** 등이 명시되어 있음. 만약 실행 환경이 프로비저닝 프로파일에 명시된 자격 조건과 맞지 않는다면 앱 실행 불가.
- 프로비저닝 프로파일은 빌드된 앱(`.ipa` 파일) 내부에서 확인 가능
-  `.ipa` 파일의 압축을 풀면 `Payload` 디렉터리 안에 `embedded.mobileprovision`란 이름의 파일(예: `Payload/sample.app/embedded.mobileprovision`)이 프로비저닝 프로파일.

#### 인증서

프로비저닝 프로파일 내 `DeveloperCertificates` 항목을 보면 [base64](https://developer.mozilla.org/en-US/docs/Web/API/WindowBase64/Base64_encoding_and_decoding)로 인코딩되어 있는 문자열을 `openssl` 명령어를 이용해 잘 확인해보면, 아래와 같이 파일로 저장된 인증서 확인 가능

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

#### Entitlements

`Entitlements` 항목에서 확인 가능. **어떤 서비스를 사용할 수 있는지**에 대한 자격 증명이 명시되어 있음. 

위 항목 내 배열(`array`)은 Xcode 빌드 설정의 **Capabilities** 탭에서 무엇을 선택했느냐에 따라 달라짐. (Ex. **Access WiFi Information** 기능과 **Apple Pay** 기능)

#### 유효 기간

`ExpirationDate` 항목에서 확인 가능. 이 항목에 명시된 유효기간이 지나면 앱 실행 불가.

#### 기기 목록

`ProvisionedDevices` 항목에서 확인 가능. 앱은 이 목록에 있는 기기에서만 실행 가능.

AD hoc Provisioning Profile (기기 제한 있음)

```xml
<key>ProvisionedDevices</key>
<array>
<string>sampleABCDEFG</string>
<string>sampleABCDEFGHIJK</string>
</array>
```

Inhouse Provisioning Profile (Enterprise로 빼낸거라 기기 제한 없음)

```xml
<key>ProvisionsAllDevices</key>
<true/>
```

([원출처](https://engineering.linecorp.com/ko/blog/ios-code-signing/#Entitlements)에서는 'Ad Hoc 등 모든 기기에서 실행 가능한 앱의 프로비저닝 프로파일에는 아래와 같이 ProvisionsAllDevices 항목이 true로 설정되어 있습니다' 라고 써있는데, 'Ad Hoc 등 모든 기기에서 실행 가능'이라는 말에 의문이 들었다. 내가 아는 한 AdHoc이 100대 제한이고,  Enterprise (InHouse)가 무제한이기 때문. 그래서 각각의 옵션으로 ipa 파일 빼낸 후 데이터를 확인해 봤는데 InHouse 의 `ProvisionsAllDevices`가 true 로 되어있었고, AdHoc은 등록된 기기의 목록이 있었다.)

## 바이너리 파일의 무결성 검증

- 서명된 앱의 **Mach-O 바이너리 파일**에는 **코드 서명과 관련된 구조체** 존재
- 해당 구조체를 이용하여 Mach-O 바이너리 파일의 무결성 검증 가능

### Mach-O 바이너리 파일

- Mach-O 바이너리 파일? 
  - Mach-O는 iOS와 macOS 계열에서 사용하는 **실행 파일 포맷**
  - iOS에서 동작하는 앱(`.ipa` 파일)의 내부에는 Mach-O 바이너리 파일이 존재하고, 해당 파일을 실행해서 앱 구동.

#### Mach-O 바이너리 파일 얻기

- `ipa` 파일의 압축을 풀면 `Payload` 디렉터리 안에 `Info.plist`라는 파일 존재(예: `Payload/sample.app/Info.plist`)
- 이 파일에서 **Executable file**이란 항목은 앱이 실행될 때 **가장 먼저 실행되는 Mach-O 바이너리 파일**을 의미

![img](https://engineering.linecorp.com/wp-content/uploads/2019/04/13-2.png)

### Mach-O 바이너리 파일 구조

![img](https://engineering.linecorp.com/wp-content/uploads/2019/04/14-1.png)

#### Mach-O 바이너리 > CodeSignature 구조 (코드 서명과 관련된 구조체)

![img](https://engineering.linecorp.com/wp-content/uploads/2019/04/18-1.png)

이 중 **CodeDirectory**와 **BlockWrapper** 구조체에 집중해보자

### CodeDirectory

- `CodeSignature` 구조체 내의 `CodeDirectory` 항목
- `CodeDirectory`는 특정 파일과 실행 바이너리 파일 조각들의 **해시값**들이 담겨져 있는 부분
- 구조체 목록 하단에서 `HashSlot` 유형의 `codeHash`와 `specialHash` 확인 가능

![img](https://engineering.linecorp.com/en/ioscodesigning19/)

#### codeHash

- 바이너리 파일을 `pageSize`(`0x1000`)만큼 나눈 부분들에 대한 **해시값**
- 따라서 **코드를 수정**하면 수정한 코드가 포함된 페이지의 **해시값 변경** 됨. 이 해시값은 `codeHash`에 저장되어 있는 해시값과는 다르기 때문에 iOS는 **코드가 수정된 사실을 인지** 가능

![img](https://engineering.linecorp.com/wp-content/uploads/2019/04/20-1.png)

### BlobWrapper

- `BlobWrapper` 안에는 [CMS(Cryptographic Message Syntax) 서명](https://tools.ietf.org/html/rfc2315) 존재

- `BlobWrapper`은 앞서 기술한 **`CodeDirectory`를 서명한 데이터**와 **데이터를 서명한 인증서**를 포함

- **`CodeDirectory`의 내용을 변경**하면 **동일한 인증서**로 다시 서명해도 **CMS 서명이 변경** 됨. 따라서 공격자가 코드를 변경한 후 `CodeDirectory`의 내용을 변경된 코드에 맞춰 수정한다고 해도 **CMS 데이터는 키를 가지고 있는 서명자 이외에는 변경할 수 없**기 때문에 **CMS 서명을 검증하면 앱의 무결성 보장** 가능.

  

## 정리

![img](https://engineering.linecorp.com/wp-content/uploads/2019/04/26-1.png)

- iOS는 바이너리 파일과 관련 파일들의 **해시값을 확인하는 것에서 끝나지 않고**, **코드 서명을 검증하여 앱의 무결성까지 확인**
- 이런 과정을 통해 변조된 앱이 임의의 사용자 기기에서 실행되는 것을 미연에 방지

**AD-Hoc 으로 배포** 시 앱이 최종적으로 디바이스에 설치될 때 아래 3가지 확인 과정 거친 후 실행.

1. .app 에 포함된 **프로비저닝 프로파일이 애플에서 서명된 것인지 확인**
2. "**CodeResources**" 란 파일에 기록된 각 파일의 해쉬 정보를 실제의 파일들과 확인하여 **빌드 후 수정이 되지 않았음을 확인**
3. 디바이스에 .app 에 포함된 **프로비저닝 프로파일이 있는지 확인**

# 출처

- [iOS 코드 서명에 대해서](https://engineering.linecorp.com/ko/blog/ios-code-signing/#Entitlements)

- [[iOS] 코드 사이닝 (프로비저닝 프로파일, 인증서)](https://beankhan.tistory.com/115)

