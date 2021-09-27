# iOS15의 Notifications interruption level (feat. Notification Summary & Focus Modes)

## 알림 요약(Notification Summary) 및 초점 모드(Focus Modes)

- Apple은 사용자가 푸시 알림과 상호 작용할 방법과 중단 시간을 결정할 수 있도록 두 가지 새로운 시스템 컨트롤을 도입

### 알림 요약

- 특정 앱에 대한 **실시간 푸시 알림 수신** 을 **중지** 할 수 있는 선택적 기능
- 대신 시스템은 이러한 앱에 대해 수신된 알림을 저장 하고 잠금 화면에 **요약** 으로 표시
- 이러한 요약은 사용자의 기본 설정에 따라 OS에 의해 하루 중 다른 순간에 트리거될 수 있음

- iOS는 알림 요약에 앱을 자동으로 추가하지 않음. 사용자가 기능을 처음 활성화할 때 시스템 설정에서 **수동으로 추가**해야 함

![iOS 15 알림 요약 설정](https://downloads.intercomcdn.com/i/o/384310747/53630a14ee133ed7748723a8/1624882536-notification-summary-test-2x.jpg)

- 알림 요약이 켜지면 푸시 권한 프롬프트가 앱에서 알림을 수신하는 두 가지 옵션을 제공
  - **즉시 알림 허용** : 사용자는 알림을 보내는 즉시 알림을 받음
  - **예약된 요약에 추가** : 앱에서 오는 알림은 나중에 알림 요약에 표시

![예약된 요약 옵트인 프롬프트](https://downloads.intercomcdn.com/i/o/384312088/a47a28efded0e2e5da67a832/Slice+2.png)

- iOS는 사용자가 선택한 푸시 옵트인 **기본 설정(즉시 알림 vs 예약 요약)을 감지하는 방법을 제공하지 않**음



### 초점 모드

- **"방해 금지"** 모드 의 개선되고 확장된 버전
- 사용자는 다른 "초점 모드"를 만들어 활동(예: 작업, 운전 등) 중에 받는 **알림** 을 **필터링** 가능 
- 시스템 설정에서 "작업" 포커스를 만들고 동료, 가족 구성원 및 특정 생산성 앱이 도착하는 즉시 알림을 받도록 설정 가능



![iOS 15 초점 모드](https://downloads.intercomcdn.com/i/o/384321409/36de8da7b6efdcc15c962616/Focus-modes-setup.png)

- 포커스 모드가 활성화되어 있는 동안 다른 앱과 관련된 알림은 다음과 같이 알림 센터에만 표시


![image](https://user-images.githubusercontent.com/20410193/134837257-3cb642b6-7f11-4742-9888-5ef7babdd028.png)

![image](https://user-images.githubusercontent.com/20410193/134837268-dc791ff0-8c8f-4ede-93f6-681147fa9c52.png)


## 새로운 알림 중단 수준 (New notifications interruption levels)

- 새로운 알림 요약 및 집중 모드 외에도 iOS 15는 푸시 알림에 대해 **Passive** 및 **Time-Sensitive** 두 가지 새로운 중단 수준을 도입
- 총 4개의 중단 수준이 있음
  - **Passive** : **즉각적인 주의가 필요하지 않은** 알림(예: 선택적 권장 사항 등).  소리/진동을 유발하거나 화면을 밝게 하지 않음.
  - **Active** : **default** 중단 수준 (예: 스포츠 업데이트, 속보 알림 등).
  - **Time-Sensitive** : **즉각적인 주의**가 필요한 알림(예: 계정 보안 문제, 패키지 배송 알림 등). 이러한 알림은 **시스템 제어(알림 요약 및 집중 모드)를 뚫을 수 있**으므로 해당 중단 수준을 사용하여 **마케팅 알림을 보내면 안됨.**
  - **Critical** : 즉각적인 주의가 필요한 **매우 중요**한 알림(예: 악천후 경보 등). 이러한 사용은 특별한 권한이 있는 **Apple에서 명시적으로 허용** 해야함. **시스템 제어(알림 요약 및 집중 모드)를 뚫을 수 있**음.

|                | sound or vibration | **screen light up** | **system breakthrough^** | **bypass ringer switch** |
| -------------- | ------------------ | ------------------- | ------------------------ | ------------------------ |
| Passive        | ❌                  | ❌                   | ❌                        | ❌                        |
| Active         | ✅                  | ✅                   | ❌                        | ❌                        |
| Time Sensitive | ✅                  | ✅                   | ✅                        | ❌                        |
| Critical       | ✅                  | ✅                   | ✅                        | ✅                        |

- **push** **notification** 같은 경우 서버에서 ` interruption-level`이라는 키값으로 payload 에 추가해서 내려야함

```
{"aps":{"interruption-level":"time-sensitive"}}
```

### Time Sensitive

- **Time-Sensitive** 레벨 알림은 **알림 요약** 과 모든 **집중 모드**를 **깨뜨릴 수 있**음 . 아래와 같이 iOS는 사용자에게 방금 수신한 알림이 Time-Sensitive 수준을 사용하여 전송되었음을 알림.

![iOS 15 시간에 민감한 알림](https://downloads.intercomcdn.com/i/o/384328460/5aca1d9965e8e866f96f20ba/Time-sensitive-alert.png)

- Time-Sensitive 알림이 **자주 interacted 되지 않**는 경우 **iOS는 잠금 화면에서 사용자에게 메시지** 를 표시하여 앱에 대한 **Time Sensitive 알림** 을 **비활성화할** 수 있음. 시스템 설정에서도 Time sensitive 알림을 비활성화할 수도 있음.



![iOS 시간에 민감한 알림 설정](https://downloads.intercomcdn.com/i/o/384327336/ac18b58f83af3a59f205abe2/time-sensitive-alerts.png)

- Time-Sensitive 알림을 사용하려면 Xcode 프로젝트에 **"Time Sensitive Notification" capability** 를 추가해야함.



![img](https://downloads.intercomcdn.com/i/o/384341353/61c41fcd9241d3bf13684ef3/CleanShot+2021-09-02+at+20.45.17%402x.png)

# 출처

- [Understanding and managing iOS 15 time-sensitive interruption level](https://help.batch.com/en/articles/5543431-understanding-and-managing-ios-15-time-sensitive-interruption-level)

- [Generating a Remote Notification](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)

