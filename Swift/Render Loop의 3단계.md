# Render Loop의 3단계

- constraint 활성화 또는 비활성화 / 상수값 변경 / priority 변경 등으로 인해 **Constraint가 변경** 될 수 있음
- Constraint 변경 시, 영향을 받는 view의 **프레임**을 **즉시 업데이트 하는 대신**, superview가 **다시 레이아웃이 필요하다고 표시**(mark) . (`super.setNeedsLayout()` / `super.setNeedsUpdateConstraints` )
- 이때 '**표시**' 란 **Deferred layout pass**가 **예약**되는 것과 같은 의미
- **Deferred Layout Pass** 는 아래의 과정들을 수행
  1. **layout의 constraints를 업데이트** 하고, 뷰의 **frame**을 **제약조건에 따라 계산** (update pass)
  2. 모든 view에 대한 **프레임을 재조정** (layout pass)
- 이렇게 **Deferred Layout Pass** (update constraints + layout) 을 거친 후에 거친 후에야 제대로 화면이 Display 될 수 있음
- **Render Loop** 는 매초 120번 정도 작동되는 프로세스로, **Update Constraints -  Layout - Display 세 단계**를 거치며 각 **프레임에 대해 모든 콘텐츠를 준비** 시킴

![image](https://user-images.githubusercontent.com/20410193/133829136-4da0f0d4-efca-481a-af1e-084151e4c696.png)

## 1. Update Constraints (Update pass)

- Auto Layout의 **제약조건(Constraints)를 업데이트** 
- 말 그대로 "제약조건을 업데이트" 한 것이라서, 실제 뷰를 배치하는데에는 영향을 **안** 줌.
- 뷰의 **frame**을 **제약조건에 따라 계산**
- 제약조건의 갱신은 뷰 계층구조 (view hierarchy)를 따라 하위 뷰에서 상위 뷰 방향으로 이루어짐. (leaf most views -> window 방향, **Bottom-Up**)
- 이렇게 Update Constraints(= Update Pass)가 끝나면 Costraints는 모두 최신상태이며 **View위치를 변경할 "준비"가 된 상태**

### 관련 함수

#### updateConstraints()

- **UIView**의 instance method로, **뷰의 제약조건을 업데이트**

- **제약 조건 변경을 최적화**할 때 **override** 해서 사용

#### updateViewConstraints()

- **UIViewController**의 instanace method로, UIView의 **`updateConstraints()` 의 뷰 컨트롤러 버전**
- 뷰 컨트롤러의 뷰가 뷰의 제약사항을 업데이트 해야 할 때 호출됨
- 제약 조건 변경을 최적화할 때 override 해서 사용

## 2. Layout (Layout pass)

- 제약조건을 바탕으로 **레이아웃(뷰의 프레임 수치 값)을 업데이트** 하는 단계
- **Update Constraints 단계에서 계산된 것들을 바탕**으로 뷰의 **프레임이 업데이트** 됨
- 이 프로세스는 전 단계와는 반대 방향. 즉, window -> leaves 방향, **Top-Down**

### 관련 함수

#### viewWillLayoutSubviews()

- **UIViewController**의 instance method

- 뷰 컨트롤러에게 **“이제 니 view가 subview들 배치할거래!”**라고 알려주기 위해 호출됨

- 뷰의 경계가 변경되면 **뷰는 하위 뷰의 위치를 조정**하는데, **그 전에 뷰 컨트롤러에게 알려주는** 것.

- 뷰가 그 뷰의 서브 뷰들을 배치하기 전에 변경사항을 주기 위해 override
  - 변경사항?
    - 뷰들을 추가하거나 제거
    - 뷰의 크기나 위치를 업데이트
    - 레이아웃 constraint를 업데이트
    - 뷰와 관련된 기타 프로퍼티들을 업데이트

#### layoutSubviews()

- **UIView**의 instance method

- Render Loop의 Layout 단계에서 제약조건을 바탕으로 레이아웃(뷰의 프레임 수치 값)을 업데이트
- 서브클래스가 그 하위 뷰들에 대해 더 정확한 레이아웃을 수행하고 싶을 때 서브클래스에서 override

#### viewDidLayoutSubviews()

- **UIViewController**의 instance method
- 뷰 컨트롤러에게 **“방금 니 view가 subview들 배치했대!”**라고 알려주기 위해 호출됨
- 뷰의 경계가 변경되면 **뷰는 하위 뷰의 위치를 조정**하는데, **그 전에 뷰 컨트롤러에게 알려주는** 것.
- 뷰가 그 뷰의 서브뷰들을 배치한 후에 변경사항을 주기 위해 override
  - 변경사항?
    - 다른 뷰들의 내용 업데이트
    - 뷰들의 크기나 위치를 최종적으로 조정
    - 테이블의 데이터를 reload
- 주의할 점은 해당 함수가 호출되었다고 해서 하위 뷰들의 개별적인 레이아웃들이 다 조정되었다는 것은 아니고, 각 하위 뷰들은 각자의 레이아웃을 조정해야 됨

## 3. 디스플레이 (Display)

- 픽셀들을 **스크린으로** 가져오는 단계
- 뷰를 Layout 단계에서 구한 수치값을 사용해 스크린에 그리는 단계
- Layout 단계와 같은 **Top - Down** 방향으로 진행

### 관련 함수

#### draw()

- **UIView**의 instance method

- 뷰에서 직사각형으로 특정된 영역에 한해 뷰를 다시 그리는 등 업데이트 할 때 호출 (매개변수로 CGRect를 받음)

- CoreGraphics나 UIKit같은 기술을 사용해서 뷰를 그리는 서브 클래스에서 drawing code를 구현하기 위해 override




# 출처

- [Render Loop을 VC, View Life Cycle과 살펴보기](https://iamcho2.github.io/2021/06/09/view-viewcontroller-layout-cycle-with-render-loop)

- [VC, View Life Cycle 함수들 알아보기](https://iamcho2.github.io/2021/06/14/vc-and-view-life-cycle-functions)

- [iOS ) Layout Cycle / The Deferred Layout Pass](https://zeddios.tistory.com/1202)

  

