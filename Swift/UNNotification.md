# UNNotification

- UILocalNotification이 iOS 10 이후로 deprecated 되면서 UNNotification이 생김. 

- 여기서 UI는 User Interface, UN은 User Notification의 약자

- 변경된 사항은 아래와 같다

  - 앱이 foreground에 있는 동안에도 alert나 sound를 재생할 수 있고 badge를 증가시킬 수도 있다
  - 앱이 죽었더라도 유저가 action button 을 클릭했을 때 모든 이벤트를 한곳에서 다룰 수 있다
  - sliding 제스처 대신 3d 터치 지원
  - just by one row code를 이용해서 특정한 local notification을 제거 가능
  - custom UI와 함께 풍부해진 알림을 지원

- UNNotification은 앱으로 날라온 local 이나 remote notification의 data

- `UNNotification` object는 알림의 payload, 날짜 등을 담고 있는 초기 알림 요청(initial notification request)을 포함 

- notification을 다룰때, 시스템이 [`UNUserNotificationCenterDelegate`](https://developer.apple.com/documentation/usernotifications/unusernotificationcenterdelegate) 에다가 알아서 notification ojbect를 전달해준다. 또한 [`UNUserNotificationCenter`](https://developer.apple.com/documentation/usernotifications/unusernotificationcenter) 는 이전에 전송된 알림들의 리스트를 저장(maintain)하기 때문에 해당 object를 얻으려면(retrieve) [`getDeliveredNotifications(completionHandler:)`](https://developer.apple.com/documentation/usernotifications/unusernotificationcenter/1649520-getdeliverednotifications)  를 사용하면 된다

- UNNotificationServiceExtension 을 사용하면 user에게 보내기 전에 remote notification의 내용을 변경할 수 있음

  `didReceive(_:withContentHandler:)` 함수가 그 역할을 함

  

# 출처

- [UNNotification](https://developer.apple.com/documentation/usernotifications/unnotification)
- [UNNotification](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dkanwk2&logNo=220857424183)
- [UILocalNotification is deprecated in iOS 10](https://stackoverflow.com/questions/37938771/uilocalnotification-is-deprecated-in-ios-10)
- [UNNotificationServiceExtension](
