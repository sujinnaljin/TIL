# FileManager를 이용해 파일 및 디렉토리 만드는 법

## 파일 생성 방법

- 파일을 만들려면, 
  1. 파일 시스템의 파일에 대한 레코드를 만들고
  2. 파일에 내용을 채워야함
- 파일을 생성하기 위한 모든 고급 인터페이스는 두 작업을 동시에 수행
- 대개 파일을 NSData 또는 NSString객체의 내용으로 채운 다음 파일을 닫음

### Detail

- **사용자에 의해 생성**되는 모든 Contents는 **Documents 디렉토리에 저장**되도록 권장됨

  파일을 하나를 어디에 저장하고 싶음 -> Documents 디렉토리 안에 저장해야 함 -> Document 디렉토리 어딨는지 파악 필요, 즉 경로 얻어야함

- **Document Directory URL** 은 **`urls(for:, in:)`** 메소드를 통해 다음과 같이 얻을 수 있음

  ```swift
  let fileManager = FileManager.default // default는 FileManager의 싱글톤 인스턴스
  let documentsURL = fileManager.urls(for: .documentDirectory, in: .userDomainMask)[0]
  ```

- 해당 **`urls(for:in:)`** 메소드는 **요청된 도메인에 지정된 공통 디렉토리의 URL 배열**을 리턴

  ```swift
  func urls(for directory: FileManager.SearchPathDirectory, 
            in domainMask: FileManager.SearchPathDomainMask) -> [URL]
  ```

  - `directory` -  **significant 디렉토리 위치**

    enum 값으로  [`FileManager.SearchPathDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory) 에서 종류들 확인 가능

    몇개만 나열해 보자면..

    - [`applicationDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/applicationdirectory) - Supported applications (`/Applications`).
    - [`developerApplicationDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/developerapplicationdirectory) - Developer applications (`/Developer/Applications`).
    - [` libraryDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/librarydirectory) - Various user-visible documentation, support, and configuration files (`/Library`).
    - [`userDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/userdirectory) - User home directories (`/Users`)
    - [`documentDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/documentdirectory) - Document directory
    - [`coreServiceDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/coreservicedirectory) - Core services (`System/Library/CoreServices`)
    - [`desktopDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/desktopdirectory) - The user’s desktop directory
    - [`cachesDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/cachesdirectory) - Discardable cache files (`Library/Caches`)
    - [`downloadsDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/downloadsdirectory) - The user’s downloads directory
    - [`musicDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/musicdirectory) - The user’s Music directory (`~/Music`)
    - [`picturesDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/picturesdirectory) - The user’s Pictures directory (`~/Pictures`)

  - `domainMask` - **significant 디렉토리를 검색할 때 사용할 기본 위치를 지정**하는 도메인. 

    enum 값으로 [`FileManager.SearchPathDomainMask`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdomainmask) 에서 종류 확인 가능

    - [`userDomainMask`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdomainmask/1408037-userdomainmask) - The user’s home directory—the place to install user’s personal items (`~`)
    - [`localDomainMask`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdomainmask/1417507-localdomainmask)  - The place to install items available to everyone on this machine.
    - [`networkDomainMask`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdomainmask/1414210-networkdomainmask) - The place to install items available on the network (`/Network`).
    - [`systemDomainMask`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdomainmask/1410500-systemdomainmask) - A directory for system files provided by Apple (`/System`) .
    - [`allDomainsMask`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdomainmask/1414912-alldomainsmask) - All domains.

- **Document Directory URL** 을 얻기 위해 `urls(for:in:)` 말고 **`url(for:in:appropriateFor:create:)`** 을 사용할 수도 있음

  해당 메소드는 **도메인에 지정된 공통 디렉토리를 찾고, 선택적으로 생성**함.

  ```swift
  func url(for directory: FileManager.SearchPathDirectory, 
           in domain: FileManager.SearchPathDomainMask, 
           appropriateFor url: URL?, 
           create shouldCreate: Bool) throws -> URL
  ```

  첫번째 두번째 파라미터는 `urls(for:in:)` 에서 설명한 것과 같음

  - `url` - 반환된 URL의 위치를 확인하는 데 사용되는 file URL. `directory` parameter는 [`itemReplacementDirectory`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/itemreplacementdirectory) 이고, `domain` parameter는  [`userDomainMask`](https://developer.apple.com/documentation/foundation/filemanager/searchpathdomainmask/1408037-userdomainmask) 가 아닌 이상 무시됨
  - `shouldCreate` -  **디렉터리가 없는 경우 해당 디렉터리를 생성할지 여부**를 판단. 임시 디렉토리를 작성할 때는 이 매개변수가 무시되고 디렉토리는 항상 작성 됨

  ```swift
  //사용 예시
  FileManager.default.url(for: .documentDirectory,
                          in: .userDomainMask,
                          appropriateFor: nil,
                          create: false)
  ```

- Documents 디렉토리의 URL을 알아냈으니 여기에 **파일을 추가**해야함 -> **파일의 이름 지정** 필요.

  **`appendingPathComponent`** 함수를 통해 file name 경로가 추가된 url을 리턴할 수 있음 (`~~/fileName`)

  만약 **특정 형식**으로 읽기까지 하고 싶다.. 하면 **확장자**도 지정 필요

  ```swift
  let fileURL = documentsURL.appendingPathComponent("fileName")
  //or if it is text..
  let fileURL = documentsURL.appendingPathComponent("fileName.txt")
  ```

- 그리고 내용을 작성 -> 비로소 파일 앱에 노출 됨

  ```swift
  let myTextString = NSString(string: "HELLO WORLD")
  try? myTextString.write(to: fileURL, atomically: true, encoding: String.Encoding.utf8.rawValue)
  ```

## 디렉토리 생성 방법

- 사용자 정의 디렉토리를 만들려면, NSFileManager의 메소드를 사용
- mkdir함수를 사용하여 직접 디렉토리를 만들 수도 있지만, 중간 디렉토리를 만들고 발생하는 모든 오류를 처리해야기 때문에 단순하게 처리 가능한  NSFileManager 메소드가 선호됨
- 경로를 만들고, NSURL또는 NSString객체를 전달하여 만들 디렉토리를 지정

### Detail

```swift
let filePath =  documentsURL.appendingPathComponent("DirectoryName")
            if !fileManager.fileExists(atPath: filePath.path) {
                do {
                    try fileManager.createDirectory(atPath: filePath.path, withIntermediateDirectories: true, attributes: nil)
                } catch {
                    NSLog("Couldn't create document directory")
                }
            }
```



# 출처

- [iOS ) FileManager를 이용해 파일/폴더 만드는 법](https://zeddios.tistory.com/440)
- [urls(for:in:)](https://developer.apple.com/documentation/foundation/filemanager/1407726-urls)
- [FileManager.SearchPathDirectory](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory)
- [FileManager.SearchPathDomainMask](https://developer.apple.com/documentation/foundation/filemanager/searchpathdomainmask)
- [url(for:in:appropriateFor:create:)](https://developer.apple.com/documentation/foundation/filemanager/1407693-url)

