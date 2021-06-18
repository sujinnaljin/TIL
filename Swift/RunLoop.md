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
  2. **Timer**  - 예약 된 시간 또는 반복 간격으로 발생하는 **동기 이벤트를 전달**

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

## 3. RunLoop를 실행시키는 방법 

- **Main Thread도 내가 RunLoop 실행시켜준 적 없는데, 왜  Timer가 정상 동작 하는 거임?** 에 대한 답은 **Main Thread는 애플리케이션이 실행될 때 프레임워크 차원에서 자동으로 RunLoop를 설정하고 실행** 하기 때문 -> 이를 **Main RunLoop** 라고 함
- Loop를 Running 시키는 방법엔 4가지 메서드 존재 

### **3-1. run()** 

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

### **3-2. run(mode:before:)**

- **루프를 한 번 실행하여, 지정된 모드에서 지정된 날짜까지 input을 blocking 한다** 

### **3-3. run(until:)**

- **지정된 날짜까지 루프를 실행하고, 그 기간 동안 루프는 부착된 모든 Input Source들의 데이터를 처리한다**

- 보통, **RunLoop를 반복 실행할 때 이 메서드를 사용**
- 이전에 부착된 Input Source들만 영구적으로 실행되는 run()과 달리,이 메서드는 **지정된 날짜**까지 루프를 실행하기 때문에, 직접 **루프의 실행 시간을 지정**해줄 수 있음
- 그리고 그 **실행 시간 동안 부착된, 모든 Input Source를 처리**해줌
-  보통 **0.1 ~ 1초 정도 RunLoop를 실행**시키고, 이를 **원할 때까지 for, while 문으로 반복**하는 형태로 작성

```swift
while isRunning {  runLoop.run(until: Date().addingTimeInterval(0.1))}
```

- 만약, 더이상 RunLoop가 필요 없어지면 어디선가 **isRunning을 false로 해주면 RunLoop가 더이상 작동하지 않음**

### **3-4. acceptInput(fotMode:before:)**

- **지정된 날짜까지 또는 지정된 모드에 대해서만 입력을 허용하여 루프를 한 번 실행한다**

## **4. 언제 RunLoop를 사용할까**

- RunLoop를 직접 사용하는 경우는, Thread를 직접 만들어서 다음과 같은 작업을 할 때
  1. **Input Source를 통해 다른 Thread와 통신하는 경우**
  2. **Timer를 사용해야 하는 경우**
  3. **Perform Selector Source를 사용해야 하는 경우**
  4. **주기적인 일을 계속 수행해야 하는 경우** 

# 출처

- [iOS) 런 루프(RunLoop) 이해하기](https://babbab2.tistory.com/68)

 
