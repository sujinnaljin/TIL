# CABasicAnimation

- **layer** 프로퍼티에 대한 기본적인 **단일 키 프레임** 애니메이션 기능을 제공하는 object

  > **key frame**
  >
  > - 영상에서 쓰이는 용어
  >
  > - 움직임에 대한 전체 프레임을 저장하지 않고, 시작점과 끝점과 같이 핵심 프레임(=key frame)만 저장하고 시작과 끝을 연결해서 영상을 만드는 것
  > - `CABasicAnimation`도 이처럼 정해진 시간동안, 시작 값에서 끝값으로 변화를 줘서 애니메이션이 이뤄짐
  >
  > **layer** 속성에 애니메이션을 적용
  >
  > - layer는 많은 속성을 가지고 있어서 다양한 애니메이션을 만들 수 있음
  >
  >   움직임이나 크기 조정 뿐 아니라 color, border, gradient, stroke 등 정말 많은 속성에 애니메이션을 줄 수 있음
  >
  > - layer는 view와 달리 단순한 model object로, 오토레이아웃이나 사용자 인터랙션과 같은 로직이 없음.
  >
  >   이로 인해 애니메이션을 적용할 때, 제약이나 고려할 점이 적어짐

- 기존에 UIKit의 요소를 애니메이션을 시키기위해선 간단하게 `UIView.animate`를 사용. 하지만 `UIView.animate(...)` 는 오직 view property animations 만 다루고, layer property animation은 다루지 않음

  따라서 **CALayer**를 애니메이션 시키기 위해선 **CABasicAnimation** 사용 필요

- `CABasicAnimation` 는 이름에서도 보이듯 `CAAnimation` 의 가장 기본적인 클래스. 

  > CAAnimation 은 Core Animation 의 abstract superclass 이며, concrete subclass 로는  [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation), [`CAKeyframeAnimation`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation), [`CAAnimationGroup`](https://developer.apple.com/documentation/quartzcore/caanimationgroup), [`CATransition`](https://developer.apple.com/documentation/quartzcore/catransition) 등이 있음

- [`init(keyPath:)`](https://developer.apple.com/documentation/quartzcore/capropertyanimation/1412534-init) 메소드의 keyPath 안에 애니메이션할 속성을 지정해서 CABasicAnimation 인스턴스를 만듦

- CABasicAnimation은 기본적인 애니메이션의 기능들을 제공. keyPath를 활용하여 특정 애니메이션을 지정하고 동작하게 할 수 있음

  더 많은 key 프로퍼티를 확인하고 싶다면 [CABasicAnimation keys](https://stackoverflow.com/questions/13913101/cabasicanimation-keys) 를 참고 

  - **opacity** : 투명도
  - **backgroundColor** : 배경 색상
  - **position** : 위치
  - **transform.scale.x** : X축으로의 크기
  - **transform.scale.y** : Y축으로의 크기
  - **transform.rotation** : 회전
  - **shadowColor**: 그림자 색상
  - **shadowOffset** : 그림자 위치
  - **shadowOpacity** : 그림자 투명도
  - **strokeEnd** : Path의 끝 부분
  - **strokeStart** : Path의 시작 부분
  - **strokeColor** : Path의 색상

- 얼마 동안, 어떻게 변화(key frame)를 줄 것인지로 애니메이션 동작을 설정할 수 있음

  시간은 `duration`(초 단위)에 지정하고, 키프레임은`fromValue`(시작값), `toValue`(끝값), `byValue`(변화량) 로 지정 가능 (`fromValue`와 `toValue` 절대적인 값이라면 `byValue`는 변화량에 대한 값)

  <img width="941" alt="image" src="https://user-images.githubusercontent.com/20410193/173192613-1cf2f07b-8cd5-4790-86cd-c408c4c77ae1.png">


  아래 예제는  `duration`: 0.3초 동안 / x 위치가 `fromValue`: 0에서 / `toValue`: 화면 끝으로 이동하게 됨

  ```swift
  // 1.애니메이션 생성
  let positionAnimation = CABasicAnimation(keyPath: "position.x")
  
  // 2.애니메이션 프로퍼티 지정
  positionAnimation.fromValue = 0
  positionAnimation.toValue = self.view.bounds.width
  positionAnimation.duration = 1.0
  
  // 3.titleLabel의 layer에 애니메이션 적용
  titleLabel.layer.add(positionAnimation, forKey: nil)
  ```

  아래와 같이`beginTime`으로 시작하는 시간을 설정할 수도 있음

  ```swift
  positionAnimation.beginTime = CACurrentMediaTime() + 0.3
  ```

  **다른 예시들**

  - [`opacity`](https://developer.apple.com/documentation/quartzcore/calayer/1410933-opacity) 와 같은 레이어의 스칼라 속성에 애니메이션 효과를 줄 수 있음. 

    **목록 1** 불투명도 애니메이션을 위한 애니메이션 만들기

    >  스칼라 - 하나의 수치(數値)만으로 완전히 표시되는 양

    ```swift
    let animation = CABasicAnimation(keyPath: "opacity") 
    animation.fromValue = 0 
    animation.toValue = 1
    ```

  - [`backgroundColor`](https://developer.apple.com/documentation/quartzcore/calayer/1410966-backgroundcolor) 같은 비스칼라 속성도 애니메이션할 수 있음. 

    **목록 2** 배경색에 애니메이션을 적용하는 애니메이션 만들기.

    ```swift
    let animation = CABasicAnimation(keyPath: "backgroundColor")
    animation.fromValue = NSColor.red.cgColor
    animation.toValue = NSColor.blue.cgColor
    ```

  - 다른 값을 사용하여 비스칼라 속성의 개별 구성 요소에 애니메이션을 적용하려면 값을 배열로 전달

    **목록 3** 위치에 애니메이션을 적용하는 애니메이션 만들기

    ```swift
    let animation = CABasicAnimation(keyPath: "position")
    animation.fromValue = [0, 0]
    animation.toValue = [100, 100]
    ```

  - 속성의 개별 구성 요소에 액세스할 수 있음. 아래 예제에서는 [`transform`](https://developer.apple.com/documentation/quartzcore/calayer/1410836-transform)  오브젝트의 x 를 1 에서 2로 애니메이션해서 레이어를 늘림

    **목록 4** 축척에 애니메이션을 적용하는 애니메이션 만들기

    ```swift
    let animation = CABasicAnimation(keyPath: "transform.scale.x")
    animation.fromValue = 1
    animation.toValue = 2
    ```

- 이렇게 만든 animation 은 `CALayer`에 `add(_anim:CAAnimation,forKey key:String?)` 메소드로 적용

  ```swift
  self.someView.layer.add(animation, forKey: nil)
  ```

- 하지만 애니메이션이 완료된 후 뷰가 원래 위치로 돌아오는데, 이는`CABasicAnimation` 가 실제 뷰가 변경되는 것이 아니라, **뷰를 비트맵 이미지로 캡처해서 이를 변경**하는 것이기 때문 

  애니메이션이 시작될 때 실제 뷰는 일시적으로 숨겨졌다가, 애니메이션이 완료되면 실제 뷰가 다시 보여짐

- 애니메이션 후에도 마지막 상태를 유지하고 싶으면 두가지 방법이 있음

  1. **애니메이션의 마지막 프레임을 유지하기**

  ```
  positionAnimation.fillMode = .forwards
  positionAnimation.isRemovedOnCompletion = false
  ```

  위 코드와 같이 `fillMode`와 `isRemovedOnCompletion`을 설정함으로써 애니메이션의 마지막 상태를 유지할 수 있음

  `fillMode`는 애니메이션 시작과 끝에 어떻게 보일지 설정하는 속성

  > **fillMode 종류 4가지**
  >
  > 1) **removed** (default): 애니메이션이 시작과 끝에만 보이고 사라짐
  >
  > ![img](https://miro.medium.com/max/1400/1*mqFEgo44vxABoOGlJ3OoFA.png)
  >
  > 2) **backwords**: 애니메이션 시작 전에, 애니메이션의 첫번째 프레임을 화면에 보여줌
  >
  > ![img](https://miro.medium.com/max/1400/1*-A9JhuDoTIo05FUU2M7o8w.png)
  >
  > 3) **forwords**: 애니메이션 완료 후에, 애니메이션의 마지막 프레임을 화면에 보여줌
  >
  > ![img](https://miro.medium.com/max/1400/1*JmbNUoWzpC5tBUDL86GMyQ.png)
  >
  > 4) **both**: backwords와 forwords를 합친 것
  >
  > ![img](https://miro.medium.com/max/1400/1*9t0MjcmQaFP-s0lOgDyxZA.png)

  이때 `fillMode` 를 `.forwords`로 하면 애니메이션의 마지막 프레임을 화면에 유지. 또한 `isRemovedOnCompletion`을 `false`로 해서 애니메이션이 사라지지 않도록 함 (기본값: `true`).

  ![img](https://miro.medium.com/max/1000/1*X-9YEFzOkGvnqzzz7V5MtQ.gif)

  유의할 점은 **애니메이션이 완료된 후에 보이는 뷰**는 **실제 뷰가 아닌, 마지막 프레임**이라는 것. 

  그러므로 화면에 남아있는 그 프레임은 사용자의 액션을 받아 인터랙션 하거나 뷰의 메소드가 제대로 사용이 안 될 것임. 

  따라서 단순히 보이 것 이상이 필요하다면, 두 번째 방법을 적용

  2. **애니메이션의 마지막 프레임과 실제 뷰를 일치시켜주기**



# 출처

- [CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)
- [[Core Animation 1] CABasicAnimation 튜토리얼](https://yungsoyu.medium.com/core-animation%EC%9D%98-%EA%B8%B0%EB%B3%B8-cabasicanimation-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC-b9f8a3b41cf7)
- [[iOS] CABasicAnimation란](https://dongminyoon.tistory.com/33)
- [Better iOS Animations with CATransaction](https://medium.com/@joncardasis/better-ios-animations-with-catransaction-72a7425673a6)
- [[iOS - swift] Shimmer 애니메이션 (CABasicAnimation, CAGradientLayer)](https://ios-development.tistory.com/932)

