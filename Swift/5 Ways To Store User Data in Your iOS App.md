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
- SQLite와 마찬가지로 데이터를 저장하는 위치가 아니라 이를 조작하는 방법. 즉, 데이터를 저장하고 관리하기 위한 프레임워크
- Core Data의 경우 해당 기기에 데이터를 저장하므로 오프라인에서도 동작 가능하며, 클라우드를 제외하고는 데이터를 공유할 수 없음
- SQL을 쓸 일 없이 오롯이 Object-Oriented 방식으로만 데이터를 다룰 수 있음. 데이터는 Object로 표현되며, 이러한 Object가 관계를 형성하여 Object Graphs를 이루고 이를 관리하는 프레임워크가 바로 Core Data
- 코어 데이터도 내부적으로는 SQL을 이용하여 데이터를 저장하지만, 개발자는 Xcode에 내장된 데이터 모델 에디터를 통해 데이터의 타입, 관계(Graphical Relationship)를 지정하고 코드로 관련 클래스를 수정할 수 있음
- 우리는 `Context`에 의해 관리되는  `DataModel` 을 만듦. 그리고 `Context`는 데이터 저장 및 검색을 담당하는 `StorePersistor` 와 상호 작용 함
- 여러 entity 및 relationship 이 있는 복잡한 object 모델을 추적해야 할 때 좋음
- 컨텍스트 및 멀티 스레딩에 대한 고급 제어 기능이 있어서 데이터 액세스를 쉽게 모듈화 할 수 있음
- 이미 모듈로 분할 된 크고 복잡한 애플리케이션을 빌드하고 있는 경우, Core Data는 좋은 선택이 될 수 있음
- 코어데이터가 SQLite보다 더 빠르게 기록을 가져올 수 있지만 더 많은 메모리와 저장 공간을 사용

### 비교
#### SQLite VS Core Data
- 보통 SQLite는 lightweight solution이 필요한 경우에, 코어 데이터는 complex object graph가 필요한 경우에 사용한다. 코어데이터가 SQLite보다 더 빠르게 기록을 가져올 수 있지만 더 많은 메모리와 저장 공간을 사용한다.
- 속도: Core Data > SQLite
- 메모리 및 저장공간 사용: Core Data > SQLite

#### Realm vs Core Data
- 설치가 쉽고 무제한 사용이 가능하다.
- Core Data 보다 빠르다.
- 서드 파티다보니 Core Data보다 앱 volume이 더 크다.

#### User Default vs Core Data
- User Default는 모든 데이터가 키/밸류 형태로 짝을 이루기 때문에 코어데이터보다 빠르지만, 말 그대로 유저 정보와 같이 작은 데이터를 저장하는데 사용된다.


# 출처

- [5 Ways To Store User Data in Your iOS App](https://betterprogramming.pub/5-ways-to-store-user-data-in-your-ios-app-595d61c89667)
- [UserDefaults, FileManger, CoreData에대한 간단한 설명](https://velog.io/@rnfxl92/UserDefaults-FileManger-CoreData%EC%97%90%EB%8C%80%ED%95%9C-%EA%B0%84%EB%8B%A8%ED%95%9C-%EC%84%A4%EB%AA%85)
