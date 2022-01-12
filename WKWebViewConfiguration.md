# WKWebViewConfiguration

- **웹 뷰**를 **초기화**하는 데 사용하는 **property**의 모음

- WKWebView 를 생성할때 [`init(frame:configuration:)`](https://developer.apple.com/documentation/webkit/wkwebview/1414998-init) 을 사용해서 WKWebViewConfiguration 을 주입 가능. 만약 해당 initializer 를 사용하지 않는다면, default configuration object 를 갖게 됨.

  ```swift
   let webConfiguration = WKWebViewConfiguration()
   webView = WKWebView(frame: .zero, configuration: webConfiguration)
  ```

- **웹 뷰는 처음 초기화 시점에만  configuration setting 을 통합**(incorporates)하기 때문에 **나중에** 해당 설정을 **동적으로 변경할 수 없음**

- `webView.configuration` 으로 웹 뷰에 대한 **configuration 세부 정보**를 확인 할 수 있는데, 이 속성은 configuration object 의 **복사본을 반환**하므로 해당 object에 대한 변경 사항은 웹 뷰의 configuration에 영향을 미치지 않음.

- 아래에서 설명할 WKWebViewConfiguration 의 property 들을 이용해서 다음의 사항들을 지정 가능

  - web content 에 사용할 수 있는 initial 쿠키
  - web content 가 사용하는 custom URL scheme의 핸들러
  - 미디어 콘텐츠 처리 방법 설정
  - 웹 뷰 내에서 선택 항목을 관리하는 방법에 대한 정보
  - 웹페이지에 삽입할 사용자 custom script
  - 컨텐츠 렌더링 방법을 결정하는 custom rule

### Configuring the Web View’s Behavior

```
// 사이트의 쿠키를 가져오고 설정하고 캐시된 데이터 개체를 추적하는 데 사용하는 개체
var websiteDataStore: WKWebsiteDataStore

// 앱의 네이티브 코드와 웹 페이지의 스크립트 및 기타 콘텐츠 간의 상호 작용을 조정하는 개체
var userContentController: WKUserContentController

// 웹 뷰가 웹 내용을 렌더링하고 스크립트를 실행하기 위해 사용하는 프로세스를 조정하는 개체
var processPool: WKProcessPool

// user-agent 문자열에 사용된 애플리케이션의 이름
var applicationNameForUserAgent: String?

// 앱 도메인 내의 페이지로 탐색을 제한할지 여부를 나타내는 Bool 값
var limitsNavigationsToAppBoundDomains: Bool
```

### Configuring the Web View’s Preferences

```
// 웹 뷰의 환경설정 관련 설정을 관리하는 개체
var preferences: WKPreferences

// 콘텐츠를 로드 및 렌더링할 때 사용할 default preferences
var defaultWebpagePreferences: WKWebpagePreferences!
```

### Adding Handlers for Custom URL Schemes

```
// 주어진 URL scheme 관련 리소스를 로드를 위한 object를 등록
func setURLSchemeHandler(WKURLSchemeHandler?, forURLScheme: String)

// 주어진 URL scheme 처리를 위해 현재 등록되어있는 핸들러를 반환
func urlSchemeHandler(forURLScheme: String) -> WKURLSchemeHandler?
```

### Configuring the Rendering Behavior

```
// 웹 뷰에서 웹 페이지 크기 변경을 허용할지 여부를 결정하는 Bool 값
var ignoresViewportScaleLimits: Bool

// 콘텐츠가 메모리에 완전히 로드될 때까지 웹 뷰에서 content 렌더링을 억제할지 여부를 나타내는 Bool 값
var suppressesIncrementalRendering: Bool
```

### Setting Media Playback Preferences

```
// HTML5 동영상이 인라인으로 재생되는지 또는 네이티브 전체 화면 컨트롤러를 사용할지 나타내는 Bool 값
var allowsInlineMediaPlayback: Bool

// 웹 뷰에서 AirPlay를 통한 미디어 재생을 허용할지 여부를 나타내는 Bool 값
var allowsAirPlayForMediaPlayback: Bool

// HTML5 동영상이 Picture in Picture을 재생할 수 있는지 여부를 나타내는 Bool 값
var allowsPictureInPictureMediaPlayback: Bool

// 재생을 하려면 사용자 제스처가 필요한 미디어 유형
var mediaTypesRequiringUserActionForPlayback: WKAudiovisualMediaTypes

// 재생을 하려면 사용자 제스처가 필요한 미디어 유형
struct WKAudiovisualMediaTypes
```

### Identifying Data Types

```
// 웹뷰의 content에 적용할 data detector type
var dataDetectorTypes: WKDataDetectorTypes

// data detector type
struct WKDataDetectorTypes
```

### Setting Selection Granularity

```
// 사용자가 웹 뷰 content를 선택할 수 있는 세분화 레벨
var selectionGranularity: WKSelectionGranularity

// 사용자가 웹 뷰 content를 선택하고 수정할 수 있는 세분화 레벨
enum WKSelectionGranularity
```

### Selecting User Interface Directionality

```
// 사용자 인터페이스 요소의 방향
var userInterfaceDirectionPolicy: WKUserInterfaceDirectionPolicy

// 웹 뷰에서 사용자 인터페이스 요소의 방향을 결정하는 정책
enum WKUserInterfaceDirectionPolicy
```

# 출처

- [WKWebViewConfiguration](https://developer.apple.com/documentation/webkit/wkwebviewconfiguration)

- [configuration](https://developer.apple.com/documentation/webkit/wkwebview/1414979-configuration)

  
