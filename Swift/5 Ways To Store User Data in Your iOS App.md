# iOS 앱에 사용자 데이터를 저장하는 5가지 방법

## 1. UserDefaults

- 정보를 저장하고 검색하는 가장 일반적이고 가장 편리한 방법
- 사용자의 기본 설정(예 : 사용자가 선호하는 모드 (dark or light), 알림 수신 여부 등)을 저장하는 데 사용.
- 일반적으로 앱에 설정 화면이있는 경우 `UserDefaults ` 가 적합

## 2. Keychain

- Keychain은 disk에 있는 특수한 파일
- hardware-encrypted 되어 있으며 접근하기 위한 low-level API 들이 많음
- 암호화하고 보안을 유지해야하는 작은 정보 (예 : 암호, 자격 증명, 토큰 등)를 저장할 때 사용
- 모든 것이 `Data`로 저장되어야 함. 따라서 해당 유형으로 직렬화 할 수 있는 object와 value만 전달해야 함.

## 3. File System

- 저장해야하는 항목이 위의 경우(UserDefault, Keychain)에 해당하지 않는 경우 파일 시스템 사용.
- 컴퓨터의 파일 시스템과 똑같이 작동하며, 경로와 URL을 사용하여 디스크의 리소스를 식별
- 암호화 해야 하지만 키 체인에 저장할 수 없는 대용량 파일의 경우 파일에 쓸 때 적절한 [암호화 옵션을](https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/encrypting_your_app_s_files) 사용하는 것이 좋음
- 웹에서 다운로드 한 이미지를 저장해서, 로딩 시간을 단축할 때는  `Library/Caches` 폴더 사용
- 사용자의 진행 상황을 저장하기 위한 파일을 `Library` 폴더에 만들 수 있음
- 앱에서 사용자가 콘텐츠를 만들 수 있도록 허용하는 경우 `Documents` 폴더 사용 (ex. 사용자가 내보낼 수있는 CSV 파일의 위치). 다만 동영상과 사진을 편집하는 경우에는 `CameraRoll` 이 더 나은 선택.

## 4. Sqlite

- [SQLite](https://www.sqlite.org/index.html) 는 내부 데이터베이스의 사실상 표준
-  `Library` 폴더는 일반적으로 SQLite 데이터베이스를 저장하기에 가장 좋은 장소
- SQLite는 데이터베이스이므로, well-structured 데이터가 많을 때 사용
- 스토리지 기능 외에도 SQL 데이터베이스가 있으면 효율적인 검색이 가능하고 복잡한 쿼리를 작성하여 흥미로운 인사이트를 얻을 수 있음

## 5. Core Data

- [Core Data](https://developer.apple.com/documentation/coredata) 는 iOS가 제공하는 [ORM (Object-Relational Mapping)](https://en.wikipedia.org/wiki/Object–relational_mapping) 프레임 워크
- SQLite와 마찬가지로 데이터를 저장하는 위치가 아니라 이를 조작하는 방법
- 우리는 `Context`에 의해 관리되는  `DataModel` 을 만듦. 그리고 `Context`는 데이터 저장 및 검색을 담당하는 `StorePersistor` 와 상호 작용 함
- 여러 entity 및 relationship 이 있는 복잡한 object 모델을 추적해야 할 때 좋음
- 컨텍스트 및 멀티 스레딩에 대한 고급 제어 기능이 있어서 데이터 액세스를 쉽게 모듈화 할 수 있음
- 이미 모듈로 분할 된 크고 복잡한 애플리케이션을 빌드하고 있는 경우, Core Data는 좋은 선택이 될 수 있음

# 출처

- [5 Ways To Store User Data in Your iOS App](https://betterprogramming.pub/5-ways-to-store-user-data-in-your-ios-app-595d61c89667)

