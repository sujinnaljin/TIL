# iOS 코드 서명 (code signing) (feat. 프로비저닝 프로파일과 Mach-O 파일)

## 코드 서명

- 코드 서명이란 **[인증서](https://github.com/sujinnaljin/TIL/blob/master/Swift/Certificate%EC%99%80%20.p12.md)를 이용해서 코드에 서명**을 하는 것.

  - 자세히는 Mach-O 파일 안의 code signature > **code directory** 를 **인증서로 서명**하면 **signature data**가 나옴 (CMS 서명). code directory에는 특정 파일과 실행 바이너리 파일 조각들의 **해시값**들이 담겨져 있음.

  ![img](https://engineering.linecorp.com/wp-content/uploads/2019/04/26-1.png)

- 이 과정을 통해 **파일의 무결성을 검증**하고 **서명자(개발자)를 확인** 가능

- Apple의 앱은 **코드 서명**을 해야 iOS 기기에서 **실행 가능**

- **코드 서명**은 반드시 **Apple에서 발급**한 **인증서**로 진행되어야 함.

  - **앱스토어**에서 설치한 앱은 **Apple의 인증서**로 코드 서명
  - **개발자**가 테스트하거나 배포하는 앱은 Apple이 발급한 **개발자 인증서**로 코드 서명

- 이때 **개발자 인증서**로 코드 서명한 앱을 기기에 설치할 때는 [**프로비저닝 프로파일**(provisioning profile)](https://github.com/sujinnaljin/TIL/blob/master/Swift/Provisioning%20profile.md) 필요.

  프로비저닝 프로파일에 명시된 기기에 프로비저닝 프로파일을 설치해야 Apple의 인증서로 코드 서명된 앱이 아니더라도 기기에서 실행 가능.

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

