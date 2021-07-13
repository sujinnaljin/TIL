

# setNeedsLayout vs layoutIfNeeded

## TL;DR;

- main run loop에서 **이벤트가 처리**되는 과정에서, **layout이나 position 값을 바꾸는 핸들러** 가 들어올 수 있음
- 시스템은 우선 이러한 **변화되는 View를 체크**한 후 **모든 핸들러가 종료**되고 **main run loop로 권한이 다시 돌아오는 시점**인 **update cycle**에서 이런 View들의 값을 바꿔주어 position이나 layout의 **변화를 적용**시킴
- 변경되어야할 레이아웃이 있으면  **update cycle** 에서는 View의 값을 **호출한 즉시 변경**시켜주는 메소드인 **`layoutSubViews()`** 가 호출됨
- **`layoutSubViews`를 유도**할 수 있는 여러 방법이 존재하는데, 이는 일종의 **update cycle에서 `layoutSubViews`의 호출을 예약**하는 행위
- **`layoutSubViews()`** 는 **비용이 많**이 들기 때문에, **직접 호출을 지양**하고 update cycle에서의 **호출을 예약** 해야 함.
- View의 **크기를 조절**할 때나, **Subview 를 추가**할때 등은 **자동**으로 `layoutSubViews`의 호출이 **예약** 됨.
- 하지만 **수동으로 호출을 예약**할 수 있는 방법도 존재하는데 그것이 **`setNeedsLayout`과 `layoutIfNeeded`**
- **`setNeedsLayout`** 를 호출한 View는 **재계산되어야 하는 View라고 수동으로 체크**가 되며 **update cycle**에서 `layoutSubviews`가 **호출** 됨. **비동기적**으로 작동하기 때문에 호출되고 **바로 반환**
- **`layoutIfNeeded`** 도 수동으로 `layoutSubviews`를 예약하는 행위이지만 update cycle 이 올때까지 기다리지 않고 **해당 예약을 바로 실행시키는 동기적으로 작동**하는 메소드
- `setNeedsLayout`과 `layoutIfNeeded`의 차이점은 **동기적으로 동작하느냐 비동기적으로 동작**하느냐의 차이



------

## Main Run Loop

- 어플리케이션이 실행되면 iOS의 `UIApplication`이 **메인 스레드에서 main run loop를 실행**시키고, main run loop는 돌아가면서 **터치 이벤트, 위치의 변화, 디바이스의 회전** 등의 **각종 이벤트들을 처리**
- 이러한 처리 과정은 **각 이벤트들에 알맞는 핸들러**를 찾아 그들에게 **처리 권한을 위임**하며 진행 (ex. 버튼의 터치 이벤트를 `@IBAction` 메소드가 처리)
- 이렇게 발생한 이벤트들을 모두 처리한 후 권한이 다시 main run loop로 돌아오게 되고 이 시점을 **update cycle**이라고 함

## Update Cycle

- main run loop에서 **이벤트가 처리**되는 과정에서, 버튼을 누르면 크기나 위치가 이동하는 애니메이션과 같이 **layout이나 position 값을 바꾸는 핸들러가 실행**될 때도 있음.

- 시스템은 이러한 **layout이나 position이 변화되는 View를 체크**한 후 **모든 핸들러가 종료**되고 **main run loop로 권한이 다시 돌아오는 시점**인 **update cycle**에서 이런 View들의 값을 바꿔주어 position이나 layout의 **변화를 적용**시킴

  ![img](https://t1.daumcdn.net/cfile/tistory/9936863F5ACA0D5C06)

- 즉 postion이나 layout 값을 **변경하는 코드**와 실제로 변경된 값이 **반영되는 시점**에는 **시간차**가 존재. 이 시간차는 짧지만, 이러한 시간차를 인지하고 있어야 정확히 원하는 핸들러를 구현 가능.

### layoutSubViews()

- View의 값을 **호출한 즉시 변경**시켜주는 메소드
- 호출되면 해당 View의 **모든 Subview들의 `layoutSubViews()` 또한 연달아 호출**. 그렇기 때문에 **비용이 많이** 들고, **직접 호출**하는 것은 **지양** 됨. 
- 시스템에 의해서 View의 값이 재계산되어야 하는 **적절한 시점(update cycle)에 자동으로 호출** 됨
- **`layoutSubViews`를 유도**할 수 있는 여러 방법이 존재하는데, 이는 일종의 **update cycle에서 `layoutSubViews`의 호출을 예약**하는 행위

## update cycle에서 `layoutSubViews` 호출을 예약

### 자동

**자동**으로 **update cycle에서 `layoutSubviews`를 호출**하는 행위에는 아래 리스트 같은 것들이 있음. 즉 시스템이 자동으로 size와 position이 변경되어야 하는 View라고 체크를 하고 update cycle에서는 `layoutSubviews`가 호출되어 체크된 View의 layer와 position에 변경된 값을 반영

- View의 크기를 조절할 때
- Subview를 추가할 때
- 사용자가 `UIScrollView`를 스크롤할 때
- 디바이스를 회전시켰을 때 (Portrait, Landscape)
- View의 Auto Layout contraint 값을 변경시켰을 때

### 수동

위의 예시처럼 자동으로 예약하는 행위 이외에도 수동으로 예약할 수 있는 메소드도 존재

#### setNeedsLayout()

- **`layoutSubviews`를 예약**하는 행위 중 가장 **비용이 적게** 드는 방법이 `setNeedsLayout`을 호출하는 것
- 이 메소드를 호출한 View는 **재계산되어야 하는 View라고 수동으로 체크**가 되며 **update cycle에서 `layoutSubviews`가 호출** 됨
- **비동기적**으로 작동하기 때문에 호출되고 **바로 반환**
- View의 보여지는 **모습은 update cycle에 들어갔을 때 바뀜**

#### layoutIfNeeded()

- 수동으로 `layoutSubviews`를 예약하는 행위이지만 **해당 예약을 바로 실행시키는 동기적으로 작동**하는 메소드
- update cycle이 올 때까지 **기다리지 않고 즉시 `layoutSubviews`를 발동**
- 만일 main run loop에서 하나의 View가 **`setNeedsLayout`을 호출하고 그 다음 `layoutIfNeeded`를 호출**한다면 `layoutIfNeeded`는 그 **즉시** View의 값이 **재계산**되고 화면에 **반영**하기 때문에 `setNeedsLayout`이 예약한 `layoutSubviews` 메소드는 **update cycle**에서 **반영해야할 변경된 값이 존재하지 않**기 때문에 호출되지 않음
- **즉시 값이 변경**되어야 하는 **애니매이션**에서 많이 사용 됨



# 출처


- [setNeedsLayout vs layoutIfNeeded](https://baked-corn.tistory.com/105)

