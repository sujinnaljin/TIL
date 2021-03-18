# Quick & Nimble
## Quick
- 기본 XCTest 도 훌륭하지만 테스트 의도를 설명하는 역할 부족함
- 의도를 설명하고 코드의 일부를 테스트하는 이유를 설명하는 데 추가로 중점을 둠
- desciription과 함께 클로저로 블럭단위 묶음으로 코딩하게끔 하여 가독성 상승을 위해 사용
- given(시나리오 정의)-when(시나리오 조건)-then(시나리오를 완료했을 때 보장되는 결과 명시)
- describe는 개념상의 묶음, context는 조건상의 묶음이고 it 블럭 안에 실제 테스트코드를 작성하게 되는 구조
```swift
    describe(“검색 필터를 클릭했을 때") {
            context(“필터 키워드를 앨범으로 선택했다면") {
                it(“앨범 API가 호출되어야하며 응답코드로 성공을 받아야 한다") {
 
                }
            }
            context(“필터 키워드를 가수로 선택했다면") {
                it(“가수 API가 호출되어야하며 응답 코드로 성공을 받아야한다.") {
 
                }
            }
         }
 
        describe(“검색어를 초기화했을 때") {
             it(“검색필터가 nil이 되어야한다”) {
             }
 
             it(“검색결과가 없습니다 라는 뷰로 갱신되어야한다”) {
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

