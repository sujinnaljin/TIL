# RunLoop

## **1. RunLoop 란**

- 소켓, 파일, 키보드 마우스 등의 **입력 소스를 처리**하는 **이벤트 처리 루프**

- 쓰레드가 일해야 할 때는 일하고, 일이 없으면 쉬도록 하는 목적으로 고안

- **Thread의 외부 입력 소스 및 Timer를 처리할 때** 등 사용됨

- **Thread**는 **모두 각자의 RunLoop**를 가질 수 있고, **Thread를 생성할 때 RunLoop가 자동으로 생성** 됨. 하지만 **자동으로 실행되진 않음**

- Main RunLoop를 제외하고 **RunLoop 실행에 대한 관리는 오롯이 "프로그래머"의 몫**

- 따라서 만약 내가 **Thread를 생성 했는데**, 이 Thread가 **입력 소스나 Timer를 처리**해야 한다면, **RunLoop를 직접 얻어서 실행**시켜 주어야 함

- 다음과 같이 현재 실행중인 스레드에 대한 런루프를 얻을 수 있음

  ```
  let runLoop = RunLoop.current
  ```

- RunLoop를 **얻는 것만으로는, 입력 소스 및 타이머를 처리해주진 않기** 때문에 **run**이라는 것을 통해 **RunLoop를 직접 실행** 시켜주어야 함

## **2. RunLoop의 작동 원리**

- RunLoop는 루프를 수행할 때, **총 2가지** **Event Source**를 수신
  1.  **Input Source** - 다른 Thread나 Application으로부터 온 **비동기 이벤트를 전달**
  2.  **Timer**  - 예약 된 시간 또는 반복 간격으로 발생하는 **동기 이벤트를 전달**

![img](https://blog.kakaocdn.net/dn/cDrIKG/btqO6h6nrTi/Ckqk0nZtAfj5eGNh08kpq0/img.png)

- 왼쪽 그림에서 **노란 색 루프를 한바퀴 도는 작업이 한 번의 실행**이라고 생각 했을 때, RunLoop는 **한 번의 실행 동안** 내 쓰레드에 **도착한 이벤트를 받고, 이에 대한 핸들러를 수행하는 객체**

- 루프라고 해서, RunLoop가 자체적으로 계속 이벤트가 들어오나 안들어오나 실행을 반복 한다고 생각할 수 있겠지만, **RunLoop는 내부적으로 반복 실행을 하지 않음**. **한 번 Event Source를 읽고 전달하는 실행이 끝나면**, 그대로 **대기**

- 그 다음 Event Source가 들어와도 RunLoop는 대기 상태이기 때문에, Event를 받을 수 없음. 따라서, 쓰레드 내에서 **프로그래머가** 명시적으로 **for, while** 등을 이용해 **RunLoop를 반복 실행**시켜 주어야 함

```swift
DispatchQueue.global.async {
	Timer.scheduledTimer(withTimeInterval: 3, repeats: false) { _ in print("")}
}
```

- 해당 코드가 동작하지 않는 이유는 Global Thread를 손수 생성 했고, 내가 생성한 **Global Thread의 RunLoop는 실행되고 있지 않기 때문**에**Timer를 작동** 시켰지만, 내 **Thread의 RunLoop가 이 Event를 처리하지 못해서**
- **Main Thread도 내가 RunLoop 실행시켜준 적 없는데, 왜  Timer가 정상 동작 하는 거임?** 에 대한 답은 **Main Thread는 애플리케이션이 실행될 때 프레임워크 차원에서 자동으로 RunLoop를 설정하고 실행** 하기 때문 -> 이를 **Main RunLoop**라고 함

## 3. RunLoop의 Evnet Source

### 3-1. Input Source

- Input Source는 이벤트를 비동기적으로 스레드에 전달

- Input Source는 두가지 타입으로 나눌 수 있다

  #### Port-Based Source

  - 애플리케이션의 Mach Port를 감시
  - Mach Port는 **커널 레벨의 기술**이기 때문에, **커널에서 자동으로 이벤트 신호**를 발생

  #### Custom Input Source

  - **사용자가 지정**한 소스의 **이벤트**를 감시
  - 다른 스레드에서 **수동으로 이벤트 신호를 발생**시킴
  - 이를 만들기 위해서는 **[CFRunLoopSource](https://developer.apple.com/documentation/corefoundation/cfrunloopsource-rhr) 의 관련함수**들을 사용하는데, Core Foundation은 이 함수들을 통해 받은 콜백 메소드를 통해 **input source를 설정**하고, **이벤트를 처리**하고, **input source를 제거**
  - 이벤트 처리 **로직 뿐아니라, 이벤트 전달 매커니즘도 정의**해야 하는데, 이 매커니즘은 **다른 스레드에서 동작하면서 input source에 데이터와 이 데이터가 준비되었음을 알리는 역할**
  - Cocoa에서는 임의의 셀렉터를 어떠한 스레드에서도 실행할 수 있도록 해주는 Custom Input Source를 NSObject 객체를 통해 제공. 이를 Perform Selector Source라고 함

### 3-2. Timer Source

- Timer Source는 **지정된 시간에 동기적으로 이벤트를 전달**
- 스레드에게 **무언가를 하도록 알림**을 주는 방법 중 하나



## 4. RunLoop를 실행시키는 방법 

- 해당 코드가 동작하지 않는 이유는 Global Thread를 손수 생성 했고, 내가 생성한 **Global Thread의 RunLoop는 실행되고 있지 않기 때문**에**Timer를 작동** 시켰지만, 내 **Thread의 RunLoop가 이 Event를 처리하지 못해서**

- **Main Thread도 내가 RunLoop 실행시켜준 적 없는데, 왜  Timer가 정상 동작 하는 거임?** 에 대한 답은 **Main Thread는 애플리케이션이 실행될 때 프레임워크 차원에서 자동으로 RunLoop를 설정하고 실행** 하기 때문 -> 이를 **Main RunLoop** 라고 함
- Loop를 Running 시키는 방법엔 4가지 메서드 존재 

### **4-1. run()** 

- **receiver를 영구 루프(parmanent loop)에 넣고, 이 기간 동안 모든 부착된 Input Source의 데이터를 처리한다**

- 아마 run()을 실행할 당시 부착되어있는 Input Source나 Timer는 영구적으로 처리한다는 의미인듯

```swift
DispatchQueue.global.async {
  let runLoop = RunLoop.current
	Timer.scheduledTimer(withTimeInterval: 3, repeats: false) { _ in print("1")} //실행
  reunLoop.run()
  Timer.scheduledTimer(withTimeInterval: 3, repeats: false) { _ in print("2")} //미실행
} 
```

### **4-2. run(mode:before:)**

- **루프를 한 번 실행하여, 지정된 모드에서 지정된 날짜까지 input을 blocking 한다** 

### **4-3. run(until:)**

- **지정된 날짜까지 루프를 실행하고, 그 기간 동안 루프는 부착된 모든 Input Source들의 데이터를 처리한다**

- 보통, **RunLoop를 반복 실행할 때 이 메서드를 사용**
- 이전에 부착된 Input Source들만 영구적으로 실행되는 run()과 달리,이 메서드는 **지정된 날짜**까지 루프를 실행하기 때문에, 직접 **루프의 실행 시간을 지정**해줄 수 있음
- 그리고 그 **실행 시간 동안 부착된, 모든 Input Source를 처리**해줌
- 보통 **0.1 ~ 1초 정도 RunLoop를 실행**시키고, 이를 **원할 때까지 for, while 문으로 반복**하는 형태로 작성

```swift
while isRunning {  runLoop.run(until: Date().addingTimeInterval(0.1))}
```

- 만약, 더이상 RunLoop가 필요 없어지면 어디선가 **isRunning을 false로 해주면 RunLoop가 더이상 작동하지 않음**

### **4-4. acceptInput(fotMode:before:)**

- **지정된 날짜까지 또는 지정된 모드에 대해서만 입력을 허용하여 루프를 한 번 실행한다**

## 5. RunLoop mode

- RunLoop 모드는 **모니터링되는 이벤트 소스**와, **알림을 받는 RunLoop 옵저버**의 집합
- RunLoop를 실행할 때 **모드를 지정**하면, 이 **모드에 해당하는 이벤트 소스만 이벤트를 보낼 수 있**다. 마찬가지로 옵저버도 모드에 해당하는 옵저버만 알림을 받을 수 있다. 
- RunLoop모드는 **이름(문자열)으로 서로 구분**되며, Cocoa와 CoreFoundation은 자주 쓰이는 모드들에 대한 프리셋을 제공
  - Default - 대부분의 연산에 대한 기본 모드
  - Modal - Modal Panel에서 발생한 이벤트만 처리. macOS에서만 사용 가능
  - Event Tracking - 특정 이벤트 루프 중에 다른 이벤트 발생을 막아야 할 때 사용 (마우스 드래깅 이벤트 등). macOS에서만 사용.
  - Tracking - 컨트롤을 추적하고 있는 상황에서 사용. iOS,tvOS에서 사용
  - Common modes - 여러 Mode의 집합. 
- 모드를 이용하면 특정 작업을 할 때 **원하지 않는 소스에서 발생한 이벤트 필터링** 가능.  
- 모드는 **이벤트가 발생하는 소스를 기준으로 구별하기 위함**이지, 이벤트의 **타입을 구별하는 용도가 아님**. 예를 들어 전체 마우스 이벤트 중에서 마우스 클릭 이벤트만 필터링 한다던지 하는 기능은 모드만으로는 구현 불가

## **6. 언제 RunLoop를 사용할까**

- **RunLoop를 직접 사용**하는 경우는, **Thread를 직접 만들어서 사용**하는 경우 뿐.

- **메인 스레드의 RunLoop**는 앱의 중요한 기반이기 때문에, **애플리케이션 프레임워크가 직접 시동 단계에서 RunLoop를 구성하고 자동으로 시작**. 절대로 메인 스레드의 RunLoop 시동 루틴을 직접 호출해서는 안됨.

- 다음과 같은 경우에 RunLoop 필요

  1. **Input Source를 통해 다른 Thread와 통신하는 경우**
  2. **Timer를 사용해야 하는 경우**

  3. **Perform Selector Source를 사용해야 하는 경우**
  4. **주기적인 일을 계속 수행해야 하는 경우** 



# 출처

- [iOS) 런 루프(RunLoop) 이해하기](https://babbab2.tistory.com/68)

- [스레드 프로그래밍(2) - RunLoop](https://jcsoohwancho.github.io/2019-09-01-%EC%8A%A4%EB%A0%88%EB%93%9C-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(2)-RunLoop/)

