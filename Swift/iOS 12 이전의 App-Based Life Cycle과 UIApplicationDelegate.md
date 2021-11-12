# iOS 12 이전의 App-Based Life Cycle과 UIApplicationDelegate

- 앱의 생명주기란 앱의 **사용과정 중 가지는 상태값**으로 크게 5개의 상태를 가짐.
- 앱의 **상태가 바뀔 때** UIKit은 적절한 **delegate object의 method를 호출**하여 우리에게 알려줌
  - **iOS 12 이하**는 life-cycle event에 응답하기 위해서 **UIApplicationDelegate** 개체를 사용
  - **iOS 13 이상**은 **UISceneDelegate** 개체를 사용하여 scene-based app에서 발생하는 life-cycle event에 응답

## 상태

![image](https://user-images.githubusercontent.com/20410193/136376091-06d83f3d-90f0-41fa-bb0d-745532c33e48.png)

### Not Running(Terminated)

- 앱이 시스템에 의해 **완전히 종료**된 상태.
- 혹은 아직 **실행되지 않았**을 때

### Inactive(Foreground) 

- 앱이 **실행 중** 이지만 **이벤트는 받지 않**는 상태
- Active 상태로 넘어가기 전에 앱은 반드시 이 상태를 거침. 
- ex. 알림 같은 특정 **알림창이 화면을 덮**어서 앱이 event를 받지 못하는 상태

### Active(Foreground)

- 앱이 **실행 중**이고 **이벤트를 받을 수 있**는 상태
- Foreground 상태에 있는 앱들은 보통 이 상태

### Background

- 앱 사용중에 **다른 앱을 실행**하거나 **홈 화면**으로 나갔을 때 상태
- **백그라운드에서 동작하는 코드**를 추가하면 suspended 상태로 넘어가지 않고 **백그라운드 상태를 유지**
- 처음부터 background 상태로 실행되는 앱은 inactive 대신 background 상태로 진입
- ex. 음악을 실행하고 **홈 화면으로 나가도 음악**이 나오는 상태

### Suspended

- 앱이 **background** 상태에서 추가적인 **작업을 하지 않으면** 곧바로 **suspended** 상태로 진입
- 시스템은 앱을 **자동으로 이 상태로 이동**하고 그렇게 하기 전에 따로 알리지(notify) 않음
- suspended 된 앱은 **메모리에 남아 있지만 코드를 실행하지 않**음
- 앱을 다시 실행할 경우 **빠른 실행을 위해 메모리**에만 올라가 있음
- **메모리가 부족**한 상황이 되면 iOS는 suspended 상태에 있는 앱들을 **메모리에서 해제**시켜서 메모리를 확보

## 상태 관련 UIApplicationDelegate method

![image](https://user-images.githubusercontent.com/20410193/136376498-d183223b-f437-42b5-8d36-3a6ee75f4010.png)


### application(_:willFinishLaunchingWithOptions:)

- 앱에 필요한 **주요 객체들을 생성**하고 **run loop를 생성**하는 등 앱의 **실행 준비가 끝나기 직전**에 호출
- App State : Launch Time(Not Running)

### application(_:didFinishLaunchingWithOptions:)

- 앱 실행을 위한 **모든 준비가 끝**난 후 화면이 사용자에게 **보여지기 직전**에 호출
- 주로 **최종 초기화 코드**를 작성
- App State : Launch Time(Not Running)

### applicationWillEnterForeground(_:)

- 앱이 Background 상태에서 **Foreground로 들어가기 직전**에 호출
- App State : Background or Not Running -> In-Active -> Active

### applicationDidBecomeActive(_:)

- 앱이 **active 상태로 진입**하고 난 직후 호출
- 앱이 실제로 사용되기 전에 마지막으로 준비할 수 있는 코드를 작성
- App State : Active

### applicationWillResignActive(_:)

- 앱이 Active에서 In-Active 상태로 진입하기 직전에 호출
- 주로 앱이 quiescent(조용한) 상태로 변환될 때의 작업을 진행
- App State : Active -> In-Active -> Background

### applicationDidEnterBackground(_:)
- 앱이 **background 상태에 진입 직후** 호출
- 앱이 Suspended 상태로 진입하기 전에 중요한 데이터를 저장하거나 점유하고 있는 공유 자원을 해제하는 등 **종료되기 전에 필요한 준비 작업**을 진행
- 앱이 종료된 이후 **다시 실행될 때 직전 상태를 복구할 수 있는 정보를 저장**
- 특별한 처리가 없으면 background 상태에서 곧바로 suspended 상태로 전환
- App State : Active -> In-Active -> Background

### applicationWillTerminate(_:)

- 앱이 **종료되기 직전에 호출**

- 사용자가 **직접 앱을 종**료할 때만 호출됨
- 아래 경우에는 호출되지 않음
  1. 메모리 확보를 위해 Suspended 상태에 있는 app이 종료될 때
  2. Background 상태에서 사용자에 의해 종료될 때
  3. 오류로 인해 app이 종료될 때
  4. Deivce를 재부팅할 때
- App State : Active -> Terminate



# 출처

- [[IOS] 어플리케이션의 생명 주기 - Life Cycle of Application](https://dev200ok.blogspot.com/2020/07/ios-application-life-cycle.html)
- [iOS Application Life Cycle](https://velog.io/@minni/iOS-Application-Life-Cycle)
- [UIApplicationDelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)
- [Managing Your App's Life Cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)

