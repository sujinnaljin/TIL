# 시뮬레이터에서 Remote Push 테스트하기

- Xcode 11.4부터 시뮬레이터에서 Remote Push Notification의 시뮬레이션을 지원하는데 두가지 방식으로 시뮬레이션가능

- 우선 아래의 코드를 통해 사용자에게 푸시 알림 사용 권한을 요청하고 허가 받아야함

  ```swift
  func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
     requestAuthorizationForRemotePushNotification()
     return true
  }
  
  // 사용자에게 푸시 권한을 요청
  func requestAuthorizationForRemotePushNotification() {
      let current = UNUserNotificationCenter.current()
      current.requestAuthorization(options: [.alert, .sound, .badge]) { (granted, error) in
      }
  }
  ```

## 1. 시뮬레이터에 apns 파일을 직접 드래그 

- 아래와 같은 .apns 확장자 파일 만들고 시뮬레이터에 드래그 & 드롭
  - Simulator Target Bundle: 시뮬레이터에 설치된 테스트 앱의 bundleID
  - aps : Remote Push Notification 사용을 위해 지정된 payload (더 많은 payload 키는 [Generating a Remote Notification](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) 에서 확인)

```json
{
    "Simulator Target Bundle" : "com.joyfulpace.apnsTest", 
    "aps" : {
        "alert" : {
            "title" : "타이틀",
            "body" : "메시지",
        },
    },
}
```

## 2. CLI 이용

- xrun 명령어 사용

```
//포맷
xcrun simctl push [시뮬레이터의 Identifier] [bundle identifier] [json file]

//예시
xcrun simctl push EEDF7FDD-6C34-4DB6-8A3B-43B71769E2D8 com.joyfulpace.apnsTest test.apns
```

- 시뮬레이터의 Identifier는 `xcrun simctl list devices` 명령으로 찾을 수 있는데, 현재 사용중인 시뮬레이터는 `Booted`라고 상태가 표시됨

```
$ xcrun simctl list devices | grep Booted
```

- CLI를 사용할 경우 bundleID가 CLI의 파라미터에 들어가기 때문에 apns 파일에 `Simulator Target Bundle` 부분 생략 가능

# 출처

- [[Xcode] 시뮬레이터에서 Remote Push 시뮬레이션하기](https://jusung.github.io/apns-test/)
- [Generating a Remote Notification](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)
- [Localize Push Notification](https://phanquanghoang.medium.com/localize-push-notification-1c907e175396)

