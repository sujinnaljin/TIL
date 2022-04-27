# UI Test

## Why need UI Test?

- 실제로 앱이랑 상호 작용하는 테스트
- 프론트엔드 또는 앱 개발을 할때는 사용자의 입력을 받게 되는데, 사용자의 입력이 제대로 처리 되는지를 테스트를 하기 위해 UI 테스트를 함
- "사용자가 어떤 workflow로 앱을 사용할 것 같은데.." -> 직접 사람이 할 수도 있지만 Xcode 에서 제공하는 UI 테스트 타겟을 사용하면 UI Test 자동화를 쉽게 구현할 수 있게 해줌
- Xcode 에서 제공하는 UI 테스트 툴은 실제 앱 타겟 내부의 코드에는 접근할 수 없지만, UI를 코드로 조작할 수 있음
- 앱의 코드를 직접 실행하는 대신 앱의 사용자 인터페이스 컨트롤을 실제 사용자처럼 사용하여 사용자가 앱을 사용하여 특정 작업을 완료할 수 있는지 여부를 결정
- 핸들링이 끝나고 결과를 확인할 때는 목표로 하는 UI 요소가 존재하는지를 검색하여 `XCTAssertNotNil` 로 판별 처리를 할 수 있음

ex) textField 에서 입력 후 버튼 누르면 해당 내용으로 alert 뜸 -> 코드로 textField 에 입력 시킨 후 버튼 누르게 하고 alert 있나 확인

![image](https://user-images.githubusercontent.com/20410193/165499352-2ce49748-1ad9-4060-aeed-e28dc09211f0.png)

![image](https://user-images.githubusercontent.com/20410193/165499387-1974ba38-4b72-431f-87a6-111ab03ef00d.png)

## UI Recording

- 음.. "textField 에서 입력 후 버튼 누르는걸 코드로 어떻게하지?!" 

  => UI Recording은 사용자의 기기, 시뮬레이터 등의 유저 인터렉션을 코드로 생성해주는 기능을 지원

- 이 기능을 활용하면 UI 테스트를 위한 테스트 코드를 만들거나, 기존의 테스트를 확장하는 데 큰 도움이 됨

- 레코딩을 하기 위해서는 테스트 메소드 안쪽에 커서를 위치시킴. 녹화 버튼은 테스트 메소드 **안에서만** 활성화 되며, 테스트 메소드 바깥에서는 비활성화.

- 녹화중인 상태에서 앱에서 어떤 행동을 하면 그게 다 코드화(?) 됨. UITest는 이 UI Recording기능을 이용하면 편하게 시작 가능. 

<img width="898" alt="image" src="https://user-images.githubusercontent.com/20410193/165499440-2de77062-bd0a-445d-8600-ae42531dc8b1.png">

<img width="959" alt="스크린샷 2022-04-27 오후 7 33 19" src="https://user-images.githubusercontent.com/20410193/165499473-8ee0ef84-f670-4939-9cc1-71b4678d1dc2.png">




- 하지만 **리팩토링이 필요하고 테스트를 검증하기 위한 코드는 직접 추가**해야 함

<img width="932" alt="image" src="https://user-images.githubusercontent.com/20410193/165499487-98cc3bd9-7dc8-4382-b4df-f00f23374d2c.png">

- 그래도 UI 테스트 코드 작성에 익숙하지 않을 때 매우 유용한 방법.

- 처음에 이 상태로 그냥 실행해보면 몇번 켜지고 꺼지고가 반복됨. 이는 `testLaunchPerformance` 에서 퍼포먼스 측정을 위해 5번 테스트를 시행한 후 평균 시간 계산하기 위함. 주석처리하면 한번만 시행.

  ```swift
  func testLaunchPerformance() throws {
          if #available(macOS 10.15, iOS 13.0, tvOS 13.0, *) {
              // This measures how long it takes to launch your application.
              measure(metrics: [XCTApplicationLaunchMetric()]) {
                  XCUIApplication().launch()
              }
          }
      }
  ```

## UI Testing API

- UITest 는 아래와 같은 이벤트와 응답으로 이뤄짐 
  1. element를 찾기 위한 쿼리를 작성
  2. element 를 탭하거나 클릭하여 응답을 유도 (cf. `button.tap()` - iOS, `button.click()` - OS X )
  3. 응답이 예상되는 결과와 일치하거나 일치하지 않는지 측정

- UITest 를 위한 UI Testing API에는 크게 3가지가 있음

### 1. XCUIApplication

<img width="978" alt="image" src="https://user-images.githubusercontent.com/20410193/165499525-fe0d2419-5157-4172-a087-be29e397b1b6.png">

- 앱을 실행 및 종료할 수 있는 앱의 "프록시"

- 앱의 시작 및 종료 lifecycle과 무관. 테스트가 별도의 프로세스에서 진행중이기 때문.

  즉 앱이 언제 시작되고 언제 종료할지 명시적으로 제어 가능

- UI 테스트를 위해서는 XCUIApplication launch함수를 호출하여 앱을 실행해주어야 하는데, `setUp` 에서 호출해주면 테스트 함수가 실행되기 직전에 매번 앱이 실행됨. (Launch는 "항상"새로운 프로세스를 생성하며 기존 인스턴스를 암시적으로 종료)

- 하나의 테스트가 끝나고 나면 원래 상태로 돌려놔야 다음 테스트를 원활하게 할 수 있기 때문에 `XCTestCase`의 `tearDown` 메소드를 오버라이딩 하여 테스트 리셋을 시킴.

  이때 테스트마다 복구하는 방법이 다를 수 있기 때문에 테스트 메소드마다 `addTeardownBlock(_:)` 함수를 사용하여 테스트 종료시 복구하는 방법을 다르게 선언해 주는 것이 가능 (*Unit Test 에서도 사용 가능* )

### 2. XCUIElement

- [XCUIElement](https://developer.apple.com/documentation/xctest/xcuielement)는 UI 테스트에서 UIButton 또는 UILabel 등의 컴포넌트를 대신하는 객체

- 주로 type 과 accessibility identifier / label 의 조합으로 element 를 찾게 됨 (ex. "titleTextField" identifier 를 가진 textField )

  아래 예시에서는 textField가 XCUIElement타입

  <img width="924" alt="image" src="https://user-images.githubusercontent.com/20410193/165499565-4d8cd8bc-4c47-4942-8ec3-f5a8a322a08c.png">

  <img width="905" alt="image" src="https://user-images.githubusercontent.com/20410193/165499599-a631ccb3-f4a3-4c2c-a0e7-26abdd93419c.png">

- XCUIApplication의 표현을 빌리자면, 앱 안의 UI element에 대한 프록시

- UI 테스트는 사람이 실제로 앱을 사용하는 것처럼 현재 실행 중인 앱에서 필요한 UI 컴포넌트를 찾아 Tap을 하거나, 텍스트를 입력하고 스크롤을 하는 등의 동작을 구현해야 함.

  따라서 Element 존재 여부인 [exists](https://developer.apple.com/documentation/xctest/xcuielement/1500760-exists), 터치를 위한 [tap](https://developer.apple.com/documentation/xctest/xcuielement/1618666-tap), 텍스트 입력을 위한 [typeText](https://developer.apple.com/documentation/xctest/xcuielement/1500968-typetext) 함수가 주로 사용하는 XCUIElement의 대표적인 Property 및 함수

  <img width="915" alt="image" src="https://user-images.githubusercontent.com/20410193/165499652-974397e3-267b-49a2-9fd1-cd6418d6c326.png">

- XCUIElement 는 XCUIElementAttributes 를 conform 하고 있는데 여기에는 value, title, label, placeholderValue, isEnabled, isSelected, frame 등이 있음.

### 3. XCUIElementQuery

- 화면에 그려진 XCUIElement를 찾기 위해서는 **[XCUIElementQuery](https://developer.apple.com/documentation/xctest/xcuielementquery)**를 사용할 수 있음.

- 즉 UI element를 찾기 위한 쿼리

  아래 예시에서는 textFields 가 XCUIElementQuery 

  <img width="935" alt="image" src="https://user-images.githubusercontent.com/20410193/165499678-d9a4961d-1564-4926-ade9-9d16c90e311d.png">

- 컴포넌트를 찾는 원리는 매우 간단. 왜냐하면 뷰는 트리 형태로 구성되어 있기 때문. 아래 그림을 참고하면 Element를 찾는 방식을 쉽게 이해 가능

  <img width="907" alt="image" src="https://user-images.githubusercontent.com/20410193/165499711-62ec028e-be28-42d5-914b-25979d307b0a.png">

- 쿼리만 쓴다고 무조건 XCUIElement 가 나오는게 아님. 아래 코드에서 **table과 cell은 여전히 XCUIElementQuery타입**

  <img width="511" alt="image" src="https://user-images.githubusercontent.com/20410193/165499738-27b75222-882e-440b-8154-d9b4bb7462f1.png">

- 위 코드는 현재 View안의 table과 cell을 전부 가져오는 코드인데,  UITest 를 위해서는 유니크한 하나의 View를 찾고 상호작용 해야하기 때문에 identifier 등을 통해 더 명확하게 쿼리를 줘야함 

  <img width="912" alt="image" src="https://user-images.githubusercontent.com/20410193/165499914-bf195679-ed8e-4eda-9f13-44c8e07abe64.png">

- query 로 element 를 찾으려면 identifier 를 이용하는 것 외에도, index 로 찾을수도 있고, element 가 unique 하다는걸 알 경우에는 그냥 element 로 찾을 수도 있음

  <img width="936" alt="image" src="https://user-images.githubusercontent.com/20410193/165499931-c7834705-6f55-4090-85bd-d35e4feec8a9.png">

- query 는 실제 사용할 때, 즉 XCUIElement 에 tap 등의 이벤트가 발생한다든가, property value 를 읽는다던가 하는 상황에서 on demand 하게 평가 (evaluated) 됨 (생성만으로 fetch 안하는 URL 과 비슷).

## UI Test = XCTest + Accessibility

- XCUIElement 를 가져오는 방법은 위에서 살펴봤듯이 다양하지만,  가장 중요한건 **Accessibility label(또는 Accessibility identifier)로 UI Element를 식별한다**는 점.

- UI Test에서의 코어 기술은 XCTest 프레임워크와 Accessibility의 조합

- Qurie 는 Accessibility 의 관점에서 visible 한 element 만 찾을 수 있음 (WWDC 2015 - UI Testing in Xcode)

  <img width="895" alt="image" src="https://user-images.githubusercontent.com/20410193/165499963-cd5d5eb1-068c-4b14-aedf-ca00f04cd054.png">

- 따라서 통합 UI 테스트 도입을 위해서는 앱의 접근성 작업이 선행되어야함. 왜냐하면 UITestRunner는 앱에서 제공해 OS에 노출된 “접근성 트리”를 활용해 버튼을 찾고 그것과 상호작용하기 때문. 

  컴퓨터에는 눈이 없으니까 당연히 시각장애인이 앱을 사용하는 것과 같은 방식으로밖에 앱을 사용할 수 없음. 즉, VoiceOver로 사용할 수 없는 버튼은 테스트코드로도 사용할 수 없음.

- Accessibility 세팅은 스토리보드 등에서 Accessibility label(또는 Accessibility identifier도 가능)을 지정 가능.

  <img width="612" alt="image" src="https://user-images.githubusercontent.com/20410193/165499990-ec9b93ee-1948-4afa-8cc9-2f74284e0013.png">

- `matching(identifier:)` 나 subscript 로 찾을 때 넘기는 값은 accessibility Identifier 나 accessibility label 둘 다 됨

  <img width="921" alt="image" src="https://user-images.githubusercontent.com/20410193/165501064-c98853c5-af78-404b-8b52-af1ba862abe3.png">
  <img width="851" alt="image" src="https://user-images.githubusercontent.com/20410193/165500049-a4df45e0-afb8-4442-be04-77c7c42d8211.png">
  

  <img width="906" alt="image" src="https://user-images.githubusercontent.com/20410193/165500083-7756b887-6679-4890-b57e-57da630eb456.png">

- 그럼에도 UI Test 는 accessibility identifier 를 이용하는게 좋음.

  왜냐하면 accessibilityLabel이 최종 사용자를 대상으로 하는 반면, accessibilityIdentifier는 개발자 전용이기 때문에 로컬라이징이 필요 없고, 되어서도 안되기 때문.

  만약 accessibilityLabel 을 기준으로 element 를 찾는다면 해당 값이 바뀌거나 다른 나라 유저가 쓰는 로컬라이징 된 앱일때 해당 element 를 못찾게 됨

- 만약 accessibility identifier, label 둘 다 없으면 기본적으로 애플에서 Accessibility label 을 눈에 보이는 text 로 설정하기 때문에 해당 값으로 찾을 수 있음

  *accessibility identifier, label 둘 중 하나라도 설정되어있을 때*

  <img width="929" alt="image" src="https://user-images.githubusercontent.com/20410193/165500166-c598ef64-9581-4db7-a602-e749db27f1b0.png">

  <img width="935" alt="image" src="https://user-images.githubusercontent.com/20410193/165500192-739c3ec6-53f8-40cc-9402-df3c4d73fbc4.png">

  *accessibility identifier, label 둘 중 모두 설정 안되어 있을 때*

  <img width="927" alt="image" src="https://user-images.githubusercontent.com/20410193/165500234-b8c1c537-c903-4573-8aff-9d6013235c8e.png">
  <img width="922" alt="image" src="https://user-images.githubusercontent.com/20410193/165500270-70c226fc-19a1-41e0-8589-2ebf72feebe2.png">


- `app.buttons[”제출”].tap()` 은 동작하지 않고, `app.otherElements[”제출”].tap()` 은 동작하는 경우가 있음

  시행착오를 거치지 않고 처음부터 적절한 XCUIElementQuery를 쓰기 위해서는 VoiceOver를 활용 할 수 있음. VoiceOver에서 그 버튼을 “버튼”이라고 불러준다면, `app.buttons` 은 동작. 만약 VoiceOver와 테스트코드에서 이 버튼을 부르는 방식을 바꾸고 싶다면 버튼의 [accessibilityTrait](https://developer.apple.com/documentation/objectivec/nsobject/1615202-accessibilitytraits)을 수정

- 만약 현재 View에 해당 Accessibility label(또는 Accessibility identifier)을 가진 요소가 없는데 tap() 등을 하려고 한다면 테스트는 실패.

  하지만 exist 를 사용할때는 있냐 / 없냐를 체크하는거기 때문에 해당 element 가 없다고 해서 무조건 실패하지는 않음

- 동일한 Accessibility label(또는 Accessibility identifier)를 가지고 있는 요소가 여러개 있어도 테스트는 실패

## 예시 코드

- 테스트 코드를 작성하기 전, **테스트 목표와 시나리오를 미리 작성**해두면 매우 큰 도움이 됨

  시나리오는 실제 앱을 사용하는 패턴으로 최대한 꼼꼼하게 작성하는 것이 좋음.  왜냐하면, 시나리오를 그대로 코드로 옮기면 테스트를 완성할 수 있기 때문

- Test함수 이름은 'test'라는 단어로 시작해야 한다는 [규칙](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/testing_with_xcode/chapters/04-writing_tests.html)이 있음 (Unit Test 와 동일)

  XCTestCase를 상속받은 테스트 클래스에 '**test**'로 시작하는 함수는 모두 테스트 함수로 인식되기 때문에 test 이후 아무렇게나 작성해도 되지만, 보통은 '테스트하려는 기능 또는 목적'을 뒤에 붙여주는 형태로 이름을 지음


### 예시 앱 1. 

- Github의 API를 이용하여 사용자를 검색하여 테이블 뷰에 보여주는 앱
- 검색어에 실시간으로 결과를 요청하고 페이지네이션을 지원
- 테이블 셀을 터치하면 상세 화면으로 이동

<img width="820" alt="image" src="https://user-images.githubusercontent.com/20410193/165500308-700aaf51-d008-46a9-8140-fdb076bcea9e.png">

#### 검색어를 입력하면, 검색 결과를 확인할 수 있는가?

1. SearchBar의 TextField를 탭
2. 검색어 입력
3. 검색 결과가 있는지 확인

```swift
func test_SearchUserResultAvailable() { 
  let searchField = app.searchFields.firstMatch 
  XCTAssertTrue(searchField.exists) // 검색창의 TextField가 있는지 확인 
  
  searchField.tap() // 1. SearchBar의 TextField를 탭 
  searchField.typeText("Mildwhale\n") // 2. 검색어 입력 후 키보드 내림 
  
  let resultCellOfFirst = app.cells.firstMatch 
  XCTAssertTrue(resultCellOfFirst.waitForExistence(timeout: 15.0)) // 3. 검색 결과가 있는지 확인 (최대 15초 동안 대기) 
}
```

#### 검색어를 삭제하면, 검색 결과가 초기화 되는가?

1. SearchBar의 TextField를 탭
2. 검색어 입력
3. 검색 결과가 있는지 확인
4. 검색어 삭제
5. 검색 결과가 있는지 확인 (없어야 한다)

#### 검색 결과 목록 중, 하나를 터치하면 상세 화면으로 이동하는가?

1. SearchBar의 TextField를 탭
2. 검색어 입력
3. 검색 결과가 있는지 확인
4. 첫 번째 셀 터치가 가능한지 확인
5. 첫 번째 셀을 터치
6. 상세 화면으로 이동하는지 확인

#### 검색 결과를 받아왔을 때, 테이블 뷰를 스크롤하면 페이지네이션이 동작하는가?

1. SearchBar의 TextField를 탭
2. 검색어 입력
3. 검색 결과가 있는지 확인
4. 테이블 뷰를 상단으로 스크롤
5. 다음 페이지가 테이블 뷰의 하단에 추가되었는지 확인

나머지 코드를 보고 싶다면 - [UITest_SearchUser.swift](https://github.com/Mildwhale/Github-SearchUser-Tutorial/blob/master/Final/GithubSearchUser-TutorialUITests/UITest_SearchUser.swift)

### 예시 앱 2. 

- 특정 아이템 번호를 넣었을 때 비동기 작업을 처리하여 테이블에 추가하는 앱
- 네비게이션 바에 있는 특정 버튼을 탭하면, 아이템 번호를 넣을 수 있는 alert 가 뜸

#### 특정 아이템 번호를 넣었을 때 비동기 작업을 처리하여 정상적으로 테이블에 추가되는가?

1. 네비게이션 바에 있는 특정 버튼을 탭

2. 노출되는 Alert 내부의 Text Field에 Text를 입력

3. 확인 버튼 탭

4. 비동기 작업을 처리하기 위해 추가 대기 시간을 주고 Expectation이 완료되는 것을 기다림

   만약 비동기 요소가 있어서 대기를 해야 할 경우가 생긴다면 유닛 테스트에서 했던 것과 유사하게 `waitForExpectation(timeout:handler:)` 또는 `wait(for:timeout:)`를 사용하면 원하는 시간만큼 테스트 흐름을 대기 시킬 수 있음

5. 테스트가 끝나면 `addTeardownBlock` 내에서 선언 한 초기화 작업을 수행하고 테스트를 종료

```swift
func testAddItemWithWriteButton() {
    let app = XCUIApplication()
    let targetText = "Item No.\(TestItemNumber)"
    
    // UI 자동 조작
    app.navigationBars["아이템 목록"].buttons["icon edit"].tap()
    
    let alert = app.alerts["아이템 번호 입력"]
    alert.collectionViews.textFields["ex) 1234567"].typeText("\(TestItemNumber)")
    alert.buttons["확인"].tap()
    
    // 비동기 작업 대기 후 체크
    let predicate = NSPredicate(format: "exists == true")
    let promise = expectation(for: predicate,
                              evaluatedWith: app.tables.cells.staticTexts[targetText],
                              handler: nil)
    wait(for: [promise], timeout: TimeoutSeconds)
    
    addTeardownBlock {
        app.tables.cells.staticTexts[targetText].firstMatch.swipeLeft()
        app.buttons["Delete"].firstMatch.tap()
    }
}
```

## What To Test

- 주로 아래와 같이 XCUIElement 의 프로퍼티인 `exists` 를 통해 element의 존재 여부 많이 판단하는듯 

```swift 
    func testAccessibilityLabel() throws {
        let myButton = app.buttons.matching(identifier: "접근성 텍스트").element
        XCTAssert(myButton.exists)
    }
```

- 아니면 exists 대신 isHittable 통해 실제로 user 가 hit 할 수 있는 element 인가 체크 (modal 등에 가리지 않고)

```
XCTAssertTrue(myButton.isHittable)
```

- 실제 디바이스(시뮬레이터)로 테스트하기 때문에 일반적으로 약간의 여유를 두는 것이 좋음. 예를 들어 애니메이션이 발생할 수 있음. 따라서 exists를 바로 사용하는 대신 waitForExistence(timeout:)를 사용하는 것이 일반적

  ```
  XCTAssertTrue(app.alerts["Warning"].waitForExistence(timeout: 1))
  ```

  해당 element가 즉시 존재하면 메소드가 즉시 반환되고 테스트가 통과되지만, 그렇지 않으면 메소드는 최대 1초까지 대기

### 의문점

#### 1. 그럼 button 의 실제 text 가 "Alert  띄우기!" 인지는 어떻게 확인하지?

<img width="373" alt="image" src="https://user-images.githubusercontent.com/20410193/165500351-fbbb6c7e-22f4-426d-96dc-566efbfeb6fe.png">

`po myButton` 찍어봤을때 Element subtree 에 accessibility label 이 "Alert 띄우기!" 인 StaticText 가 있다는 것 확인

<img width="929" alt="image" src="https://user-images.githubusercontent.com/20410193/165500418-0f388af4-f357-4d9e-a499-a611997ac2dc.png">

그럼 `myButton` 의 하위 staticTexts 타입들 중에 "Alert 띄우기!" accessibility label 으로 접근 가능한 element 가 있는지 보면 되겠다!

<img width="938" alt="image" src="https://user-images.githubusercontent.com/20410193/165500443-613708ff-f3af-4c92-b2cc-c62a87f3bc9e.png">

그럼 뭐.. 이런식으로 가능할 듯

```swift
XCTAssert(myButton.staticTexts["Alert 띄우기!"].exists)
XCTAssertEqual(myButton.staticTexts.element.label, "Alert 띄우기")
```

하지만 많은 사람들이, “주어진 조건에서 원하는 정보가 원하는대로 출력되는지”를 테스트하기 위해 “ViewModel”을 테스트하라고 조언. ViewModel에서 출력이 기대한대로 나오면, ViewModel과 View가 제대로 연결되어있다는 가정 하에, View에는 기대한 출력이 그대로 반영될 것이기 때문. 따라서 이런 값 할당 등의 확인은 Unit Test 가 난듯.

#### 2. 그럼 background 색상 테스트도 할 수 있나?

질문:  "Requested -> 회색, Draft -> 노란색 인거 UITest 로 체크할 수 있냐?" or "view 의 background 색상 확인할 수 있냐?"

<img width="409" alt="image" src="https://user-images.githubusercontent.com/20410193/165500470-335e8cae-8731-45b6-b874-59e7becf7513.png">

답변: 유감.

UI 테스트는 접근성이 지원하는 볼 (see) 수 있는 범위에서만 인터페이스를 확인할 수 있음. 

따라서 텍스트를 "볼" 수는 있지만 이 항목이 UILabel 인 것을 "볼" 수는 없음. 따라서 UILabel로서의 특성을 탐색할 수 없음

비슷한 맥락으로 백그라운드 색상 확인도 불가능.

따라서 색상 확인하고 싶으면 Unit Test 를 사용해서 뷰를 초기화하고 배경색을 확인하든가, UITest 에서 screenShot 찍어서 직접 확인하든가..?

![image](https://user-images.githubusercontent.com/20410193/165500516-7a3f925f-fb3d-4d4e-9ce1-218d0713e088.png)

**참고**

[UITest color of a label (not UI label)](https://stackoverflow.com/questions/37754696/uitest-color-of-a-label-not-ui-label)

[How to check background color of the XCUIElement in UI test cases??](https://stackoverflow.com/questions/65590132/how-to-check-background-color-of-the-xcuielement-in-ui-test-cases)

[In XCTest UI Testing, how to check the background color of button, label , views?](https://stackoverflow.com/questions/38667435/in-xctest-ui-testing-how-to-check-the-background-color-of-button-label-views)

## Parallel UI Testing

- **Xcode 9 부터 병렬 테스트를 지원** 해서 동시에 2개 이상의 기기에서 테스트 진행 가능

- UI 테스트에 사용하는 시뮬레이터의 개수는 시스템의 성능에 따라 자동으로 결정

- 터미널 명령어 xcodebuild로 Runner(worker) 의 개수를 직접 설정할 수도 있지만, 너무 많은 개수를 지정하면 시뮬레이터를 실행하는 데 모든 자원을 사용해버려서 테스트가 진행되지 않음. 빌드머신이 감당할 수 있는 최대의 개수를 지정해주는 것이 가장 좋음.

  ```sh
  -parallel-testing-worker-count n 
  ```

- 병렬화 테스트는 Class 단위로 테스트를 진행. 즉, 하나의 Class에 너무 많은 테스트 함수가 구현되어 있다면 이것을 적절히 나누어 주는 것이 좋음.  시뮬레이터는 3개인데 테스트 함수가 1개의 Class에 모두 구현되어있다면, 병렬 테스트의 장점을 전혀 살리지 못함

<img width="943" alt="image" src="https://user-images.githubusercontent.com/20410193/165500551-d8952bb2-7ef1-4952-b174-0b31b7b82747.png">

1. Edit Scheme 메뉴 진입
2. Test 메뉴 선택
3. 병렬화하고자 하는 Tests의 `Options...` 선택
4. `Execute in parallel on Simulator`에 체크

 ## When To Use

- UI 테스트는 앱이 예상대로 사용자에게 작동하는 궁극적인 지표이지만 다른 종류의 테스트보다 실행하는 데 시간이 더 오래 걸림

  *프로젝트에서 목표로 하는 단위, 통합 및 UI 테스트의 상대적인 양을 보여주는 그림*
  <img width="955" alt="image" src="https://user-images.githubusercontent.com/20410193/165500602-f724debd-df29-4b88-9a68-b840806cf6b1.png">


- 동일한 UI 테스트에서 실패를 유발할 수 있는 다양한 앱 변수가 있음 (아니.. 버튼 위치 가린다고 안됐다니까?)

  예를 들어 아래처럼 레이아웃을 맞춰놨는데, se 같은 작은 기기에서는 키보드가 "alert 띄우기!" 버튼을 가릴 수 있음. 

  ![image](https://user-images.githubusercontent.com/20410193/165500770-022e31aa-57db-407b-a7ff-860a74b90663.png)

  ![image](https://user-images.githubusercontent.com/20410193/165500792-7fdd0dd2-e254-4a43-9959-491c248df0c2.png)

  

  그 상태에서 해당 버튼을 .tap() 하려고 한다면, 탭할 수 없다고 에러남;;

  ![image](https://user-images.githubusercontent.com/20410193/165500829-b0275745-76d4-4ed4-9927-adbd912863c7.png)

  UI Test 실패하면 실패 상태를 Report Navigator 에서 screen shot 으로 볼수 있음

  <img width="925" alt="image" src="https://user-images.githubusercontent.com/20410193/165500855-e577cb7a-f514-4955-a47b-b90e1863b5ec.png">

  아니면 textField 에 h.tap(), i.tap() 을 한다고 작성해뒀는데, 기본 자판이 한글이라 테스트가 실패할 수도 있음

- 이처럼 통합 UI테스트의 유지 보수 비용은 비쌈. 

- 실제 앱을 대상으로 하는 통합UI 테스트는 당연히 서버에 API를 쏠 때도 진짜 서버에 쏘기 때문에 실패할 여지가 굉장히 많고 그만큼 다루기 어려움. 

- 그렇기 때문에 통합 UI 테스트만으로 앱의 모든 기능을 구석구석 수시로 테스트하는 것은 비현실적. 하지만 반드시 꼼꼼하고 확실하게 테스트해야 하는 영역, “이 기능이 망가지면 회사가 망한다”고 생각되는 영역들에 대해서 만큼은, 통합 UI테스트를 작성해야만 함.

# 출처

[UITest (1) - UITest란?](https://zeddios.tistory.com/1061)

[UITest (2) - Recording UI Test](https://zeddios.tistory.com/1062)

[UITest (3) - XCUIApplication / XCUIElement / XCUIElementQuery](https://zeddios.tistory.com/1064)

[accessibilityLabel / accessibilityIdentifier](https://zeddios.tistory.com/1094)

[[iOS] Xcode를 이용한 UI 테스트 - 1. 시작하기](https://mildwhale.tistory.com/32 )

[[iOS] Xcode를 이용한 UI 테스트 - 2. 테스트 케이스 작성](https://mildwhale.tistory.com/33 )

[[iOS] Xcode를 이용한 UI 테스트 - 3. 테스트 녹화](https://mildwhale.tistory.com/34?category=737132)

[[iOS] Xcode를 이용한 UI 테스트 - 4. Tip & Tricks for UI Testing](https://mildwhale.github.io/2019-12-26-uitesting-tip-and-tricks/)

[[iOS\] Xcode에서 비동기 작업 테스트하기](https://mildwhale.github.io/2019-11-22-async-test/)

[Github-SearchUser-Tutorial.swift](https://github.com/Mildwhale/Github-SearchUser-Tutorial/blob/master/Final/GithubSearchUser-TutorialUITests/UITest_SearchUser.swift)

[[iOS] Parallel UI Testing](https://mildwhale.tistory.com/35?category=737132)

[[Unit / UITest\] Parallel Testing으로 Test시간을 줄여보자](https://eunjin3786.tistory.com/298)

[뱅크샐러드 iOS팀이 숨쉬듯이 테스트코드 짜는 방식 1편 - 통합 UI테스트](https://blog.banksalad.com/tech/test-in-banksalad-ios-1/)

[아, 나도 테스트 코드 짠다! - (2) Xcode UI 테스트 작성](https://blog.dehol.kr/lets-make-test-code-2-ui-test/)

[Xcode UI Testing Cheat Sheet](https://www.hackingwithswift.com/articles/148/xcode-ui-testing-cheat-sheet)

[뱅크샐러드 iOS팀이 숨쉬듯이 테스트코드 짜는 방식 2편 - 화면 단위 통합 테스트](https://blog.banksalad.com/tech/test-in-banksalad-ios-2/)

**애플 공식 문서**

[Testing Your Apps in Xcode](https://developer.apple.com/documentation/xcode/testing-your-apps-in-xcode)

[User Interface Testing](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/testing_with_xcode/chapters/09-ui_testing.html#//apple_ref/doc/uid/TP40014132-CH13-SW5)

[XCUIElementAttributes](https://developer.apple.com/documentation/xctest/xcuielementattributes)

[Recording UI Tests](https://developer.apple.com/library/archive/documentation/ToolsLanguages/Conceptual/Xcode_Overview/RecordingUITests.html)

**테스팅 관련 WWDC**

[WWDC 2015 - UI Testing in Xcode](https://developer.apple.com/videos/play/wwdc2015/406/)

[WWDC 2019 - Testing in Xcode](https://developer.apple.com/videos/play/wwdc2019/413/)

[WWDC 2018 - What's New in Testing](https://developer.apple.com/videos/play/wwdc2018/403/)

[WWDC 2017 - What's New in Testing](https://developer.apple.com/videos/play/wwdc2017/409/)

[WWDC 2017 - Engineering for Testability](https://developer.apple.com/videos/play/wwdc2017/414/)

[WWDC 2018 - Testing Tips & Tricks](https://developer.apple.com/videos/play/wwdc2018/417/)

[WWDC 2020 - Handle interruptions and alerts in UI tests](https://developer.apple.com/videos/play/wwdc2020/10220/)

