# Quick & Nimble
## Quick
- 기본 XCTest 도 훌륭하지만 테스트 의도를 설명하는 역할 부족함
- 의도를 설명하고 코드의 일부를 테스트하는 이유를 설명하는 데 추가로 중점을 둠
- desciription과 함께 클로저로 블럭단위 묶음으로 코딩하게끔 하여 가독성 상승을 위해 사용
- given(시나리오 정의)-when(시나리오 조건)-then(시나리오를 완료했을 때 보장되는 결과 명시)
- describe는 개념상의 묶음, context는 조건상의 묶음이고 it 블럭 안에 실제 테스트코드를 작성하게 되는 구조
### Describe 
- 어떤 component를 test 하는지 설명한다. 
- ex) Vehicle
- Describe는 테스트 대상을 명사로 작성한다.

### Context
- test의 목적이나, object의 현재 state를 설명한다. 
- ex) Car 로 초기화 되고 난 후 (You can and definitely should create tests that will test the inappropriate state of an object.)
- 영어로 Context문을 작성할 때에는 반드시 with 또는 when으로 시작하도록 한다.
- 한국어로 한다면 ~인 경우, ~할 때, 만약 ~ 하다면 과 같이 상황 또는 조건을 기술한다.
### It
- test에서 기대되는 결과를 설명한다. 위에서 명사로 작성한 테스트 대상의 행동을 작성한다.
- ex) 몰 수 있어야한다.
- It 구문은 It returns true, It responses 404와 같이 심플하게 설명할수록 좋다.
- 테스트 대상의 행동은 ~이다, ~한다, ~를 갖는다가 적절. 수동형은 적절하지 않음
```swift
// Swift

import Quick
import Nimble

class DolphinSpec: QuickSpec {
  override func spec() {
    describe("a dolphin") {
      var dolphin: Dolphin!
      beforeEach { dolphin = Dolphin() }

      describe("its click") {
        context("when the dolphin is not near anything interesting") {
          it("is only emitted once") {
            expect(dolphin.click().count).to(equal(1))
          }
        }

        context("when the dolphin is near something interesting") {
          beforeEach {
            let ship = SunkenShip()
            Jamaica.dolphinCove.add(ship)
            Jamaica.dolphinCove.add(dolphin)
          }

          it("is emitted three times") {
            expect(dolphin.click().count).to(equal(3))
          }
        }
      }
    }
  }
}
```


## Nimble
- 여러가지 종류의 assertion 제공
- 에러 로그 직접 입력할 필요 없고, 에러 로그를 명확히 보여줌 

# 출처
- [iOS 어썸블로그 앱 제작기](https://brunch.co.kr/@tilltue/38)
- [[Nimble] Unit Test 라이브러리 - Nimble](https://eunjin3786.tistory.com/81)
- [[iOS - swift] Nimble, Quick 프레임워크 (Unit test)](https://ios-development.tistory.com/338)
- [[Test-Concept] 테스트 왜 필요하고 해야하는가?!](https://eunjin3786.tistory.com/87?category=809811)
- [Quick documentation](https://github.com/Quick/Quick/tree/master/Documentation/ko-kr)
- [TDD using Quick & Nimble](https://medium.com/inloopx/tdd-using-quick-nimble-244b14b09e3d)
- [JUnit5로 계층 구조의 테스트 코드 작성하기](https://johngrib.github.io/wiki/junit5-nested/)
- [Quick Example과 Example그룹으로 구성된 테스트](https://github.com/Quick/Quick/blob/master/Documentation/ko-kr/QuickExamplesAndGroups.md)

