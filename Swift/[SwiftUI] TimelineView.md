# [SwiftUI] TimelineView

- **제공 된 스케쥴에 따라 업데이트** 되는 view (iOS 15.0+)

  ```swift
  TimelineView(.periodic(from: startDate, by: 1)) { context in AnalogTimerView(date: context.date)}
  ```

- content를 작성하는 클로저는 [`TimelineView.Context`](https://developer.apple.com/documentation/swiftui/timelineview/context) type의 input을  받음

  - context 에는 **업데이트를 트리거 한 [`date`](https://developer.apple.com/documentation/swiftui/timelineview/context/date)** 가 포함되어 content 의 appearance를 customize 할 수 있음.

  - context 에는 타임라인이 **view를 업데이트하는 속도인 [`cadence`](https://developer.apple.com/documentation/swiftui/timelineview/context/cadence-swift.property)**  프로퍼티도 포함되어 있음. 이를 통해 불필요한 세부 정보를 숨기는 데 사용할 수 있는데, 예를 들어 타이머의 초침을 표시하는 것이 적절한지 결정 가능

    ```swift
    TimelineView(.periodic(from: startDate, by: 1.0)) { context in    
                                                       AnalogTimerView(date: context.date, 
                                                                       showSeconds: context.cadence <= .seconds)}
    ```

- [`TimelineSchedule`](https://developer.apple.com/documentation/swiftui/timelineschedule)  프로토콜을 준수하는 type을 생성해서 custom schedule을 정의하거나, 다음과 같은 기본 제공 schedule 중 하나를 사용할 수 있음

  - [`everyMinute`](https://developer.apple.com/documentation/swiftui/timelineschedule/everyminute) - **매 분**마다 업데이트할 때 사용 

    ```swift
    TimelineView(.everyMinute) { context in }
    ```

  - [`periodic(from:by:)`](https://developer.apple.com/documentation/swiftui/timelineschedule/periodic(from:by:)) **사용자 지정 시작 시간과 업데이트 간격**에 따라 주기적으로 업데이트할 때 사용

    ```swift
    TimelineView(.periodic(from: Date(), by: 1)) { context in }
    ```

  - [`explicit(_:)`](https://developer.apple.com/documentation/swiftui/timelineschedule/explicit(_:))  - 제한된 수 또는 불규칙한 업데이트 set 이 필요할 때 사용

    ```swift
    TimelineView(.explicit(getDates())) { context in }
                                         
    private func getDates() -> [Date] { 
        let date = Date( ) 
        return [date, 
                date.addingTimeInterval(1.0), 
                date.addingTimeInterval(2.0), 
                date.addingTimeInterval(3.0), 
                date.addingTimeInterval(4.0), 
        ] 
    } 
    ```

- **과거 날짜만 포함**하는 스케줄의 경우 timeline view에 스케줄의 **마지막 날짜**가 표시 됨. **미래의 날짜만 포함**하는 스케줄의 경우 타임라인은 첫 번째 예약된 **날짜가 도래할 때까지 현재 날짜를 사용**하여 내용을 표시.



# 출처

- [TimelineView](https://developer.apple.com/documentation/swiftui/timelineview)
- [TimelineView in SwiftUI 3 and iOS 15](https://devtechie.medium.com/timelineview-in-swiftui-3-and-ios-15-244edc91a91d)

