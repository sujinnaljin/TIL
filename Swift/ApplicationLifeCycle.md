# iOS의 애플리케이션 수명주기

## 기기 재부팅부터 앱 실행까지

- 사용자가 폰을 켰을 때 OS에 속한 앱을 제외하고는 어떤 앱도 실행되지 않고, 사용자가 **앱 아이콘을 탭**하면 **SpringBoard**가 **앱을 실행**

> SpringBoard는 iPhone의 홈 화면을 관리하는 표준 애플리케이션. 기타 작업에는 WindowServer 시작, 응용 프로그램 시작 및 부트 스트랩, 시작시 장치 설정 설정 등이 포함.

- **스프링 보드**가 앱의 시작 화면에 애니메이션을 적용하는 동안 앱과 필요한 공유 라이브러리가 메모리에 로드됨. 이후 앱이 실행을 시작하고 **애플리케이션 대리자(delegate)가 알림**을 받음.

- **AppDelegate**는 응용 프로그램 위임 object. **UIResponder 클래스**를 상속하고 **UIApplicationDelegate 프로토콜**을 구현.

  - **UIApplicationDelegate** 는 iOS 앱의 **main entry**. AppDelegate가 **애플리케이션의 수명주기를 관리**하고 응답할 수 있도록 함. 즉, 앱 실행, 앱이 백그라운드 또는 포그라운드로 이동, 앱 종료, 푸시 알림 등과 같은 사용자 이벤트에 대한 알림 처리. 

  - **UIResponder** 는 AppDelegate가 **사용자 이벤트에 응답** 할 수 있도록 함

    

## 앱 실행 상태

1. **Not Running** : 앱이 시스템에 의해 시작되거나 종료되지 않은 상태

2. **Inactive** : 앱이 foreground 상태로 들어가지만 이벤트를 수신하지 않는 상태

3. **Active** : 앱이 foreground 상태가 되어 이벤트를 처리 할 수 있는 상태

4. **Background** : 이 상태에서 실행 코드가 있으면 실행하고, 실행 코드가 없거나 실행이 완료되면 suspended 로 전환

5. **Suspended** : 앱이 백그라운드 (메모리) 에 있지만 코드를 실행하지 않고 시스템에 메모리가 충분하지 않은 경우 앱이 종료

   

## launch부터 suspended 상태까지의 앱 수명 주기 흐름

![img](https://miro.medium.com/max/2480/1*vodkKQtyPvtYr1PmRgnJ0w.png)



# 출처

- [Application life cycle in iOS](https://manasaprema04.medium.com/application-life-cycle-in-ios-f7365d8c1636)

