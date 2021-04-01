# UITest

- UITest에서 코어 기술은 **XCTest프레임워크와 Accessibility의 조합**이기 때문에 좋은 UITest를 작성하려면 **accessibilityIdentifier** 작업이 되어있어야 함. (따라서 접근성 지원 잘 해두면 UITest 용이)

- 보통 **UI Recording** 기능을 이용해서 러프한 UI Test 코드를 작성. 하지만 복잡한 구조에서는 해당 기능이 엄~청 느리게 동작하는 element들이 있는 듯

- 처음에 그냥 실행해보면 몇번 켜지고 꺼지고가 반복된다. 이는 `testLaunchPerformance` 에서 퍼포먼스 측정을 위해 5번 테스트를 시행한 후 평균 시간 계산하기 위함. 주석처리하면 한번만 시행된다.

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

  앱 런치하는 코드를 setup 쪽으로 옮기고 testExample으로 커서를 옮겨서 레코딩을 시행한다

  ```swift
  override func setUpWithError() throws {
          continueAfterFailure = false
  
          let app = XCUIApplication()
          app.launch()
      }
  
  func testExample() throws {
      }
  ```

- 각 테스트 func 을 하나의 class에서 작성해서 run 하면 시뮬레이터 한개에서 돌아가지만, 여러개 class로 나눠서 테스트 코드 작성하고 Edit Scheme에서 Execute in parallel on Simulator 체크해주면 **병렬 테스트** 가능 ([참고](https://mildwhale.tistory.com/35))

- 여러 개의 앱 테스트를 번들 아이디 이용해서 하나의 UI test 코드에서 가능

- `firstMatch` 가 메모리랑 타임 퍼포먼스 측면에서 도움이 되니 잘 쓰자. 하지만 용도 알고 잘 써야함

  app.navigationBars.buttons["Done"] 해도 tableView 안의 button 까지 다 찾는다 함;; (지금은 안그러려나?)

## UITestCase 예시

```swift
class MyAppUITests: XCTestCase {

    let app = XCUIApplication()
    var topScrollButton: XCUIElement {
        app.buttons["위로 가기"]
    }
    
    override func setUpWithError() throws {
        continueAfterFailure = false
        app.launch()
    }

    override func tearDownWithError() throws {
        
    }
    
    // TestCase 1. 스크롤을 하면, 위로가기 버튼 나타는가?
    func test_TopButtonAvailableWhenScroll() {
        swipeUpScrollView()
        XCTAssertTrue(topScrollButton.waitForExistence(timeout: 3.0)) //스와이프 했을때 늦게 뜰 수도 있으니까 기다려줌
    }
}

extension MyAppUITests {
    private func swipeUpScrollView() {
        let firstTableViewCell = app.tables.cells.firstMatch
        XCTAssertTrue(firstTableViewCell.waitForExistence(timeout: 10.0)) //비동기니까 10초까진 기다려줌
        firstTableViewCell.swipeUp()
    }
}
```



# 참고

- [UI Testing in Xcode (in WWDC 2015)](https://developer.apple.com/videos/play/wwdc2015/406/)
- [UITest (1) - UITest란?](https://zeddios.tistory.com/1061)
- [UITest (2) - Recording UI Test](https://zeddios.tistory.com/1062)
- [UITest (3) - XCUIApplication / XCUIElement / XCUIElementQuery](https://zeddios.tistory.com/1064)
- [accessibilityLabel / accessibilityIdentifier](https://zeddios.tistory.com/1094)
- [iOS 단위 테스트와 UI 테스트 튜토리얼 (iOS Unit Testing and UI Testing Tutorial)](https://kka7.tistory.com/68)
- [[iOS\] Xcode를 이용한 UI 테스트 - 1](https://mildwhale.github.io/2019-11-30-uitest-1-beginning/)
- [[iOS\] Xcode를 이용한 UI 테스트 - 2](https://mildwhale.github.io/2019-12-01-uitest-2-testcase/)
- [[iOS\] Xcode를 이용한 UI 테스트 - 3](https://mildwhale.github.io/2019-12-03-uitest-3-record/)
- [[iOS\] Xcode를 이용한 UI 테스트 - 4](https://mildwhale.github.io/2019-12-26-uitesting-tip-and-tricks/)
- [[iOS\] Parallel UI Testing](https://mildwhale.tistory.com/35)

- [Recording UI Tests](https://developer.apple.com/library/archive/documentation/ToolsLanguages/Conceptual/Xcode_Overview/RecordingUITests.html)
- [User Interface Testing](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/testing_with_xcode/chapters/09-ui_testing.html)
- [Writing Test Classes and Methods](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/testing_with_xcode/chapters/04-writing_tests.html)
- [descendants(matching:)](https://developer.apple.com/documentation/xctest/xcuielementquery/1500659-descendants)
- [children(matching:)](https://developer.apple.com/documentation/xctest/xcuielementquery/1501006-children)
- [containing(_:identifier:)](https://developer.apple.com/documentation/xctest/xcuielementquery/1500995-containing)
- [아, 나도 테스트 코드 짠다! - (2) Xcode UI 테스트 작성](https://blog.dehol.kr/lets-make-test-code-2-ui-test/)
- [[iOS\] Xcode에서 비동기 작업 테스트하기](https://mildwhale.github.io/2019-11-22-async-test/)

- [[iOS\] Xcode를 이용한 UI 테스트 - 2. 테스트 케이스 작성](https://mildwhale.tistory.com/33)
- [Github-SearchUser-Tutorial.swift](https://github.com/Mildwhale/Github-SearchUser-Tutorial/blob/master/Final/GithubSearchUser-TutorialUITests/UITest_SearchUser.swift)

- [What's New in Testing](https://developer.apple.com/videos/play/wwdc2017/409)

