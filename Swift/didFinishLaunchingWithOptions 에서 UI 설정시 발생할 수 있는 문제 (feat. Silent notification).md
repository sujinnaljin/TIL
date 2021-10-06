# didFinishLaunchingWithOptions 에서 UI 설정시 발생할 수 있는 문제 (feat. Silent notification)

- 만약 **앱이 종료된 상태**에서 **Silent notification 을 수신**하면 알림을 전달하기 위해 백그라운드에서 앱을 깨우고 AppDelegate의 **delegate 메서드를 호출**

  1. **`willFinishLaunchingWithOptions`**
  2. **`didFinishLaunchingWithOptions`**

- 따라서 아래와 같이 **`didFinishLaunchingWithOptions`** 에서 **UI 설정**을 할 경우, `content-available` notification이 오는 경우에 (**Silent notifications** for background fetching) **문제가 발생**할 수 있음 (여기서 VC를 초기화하기 때문)

  ```swift
  func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
      // Initalize data-structures, prepare for notifications, check launchOptions for routing if available
      window = UIWindow(frame: UIScreen.main.bounds)
      window?.tintColor = R.color.purple()
      window?.backgroundColor = R.color.cloudBlue()
      window?.rootViewController = UIViewController()
      window?.makeKeyAndVisible()
      return true
  }
  ```

- **`didFinishLaunchingWithOptions`** 에서는 아래와 같은 **일회성 설정**을 수행하며,  긴 작업을 실행하지 않아야함 

  - 데이터 구조를 초기화
  - 필요한 리소스를 확인
  - 알림 준비
  - 라우팅을 위한 시작 옵션 확인
  - 타사 SDK 초기화

- 대신 foreground 에서 실행할 **UI 준비는 `applicationDidBecomeActive`** 에서 작업. 이는 **종료 또는 백그라운드 상태에서 포그라운드 상태로 전환**될 때 호출됨

  ```swift
  func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
      // Initalize data-structures, prepare for notifications, check launchOptions for routing if available
      window = UIWindow(frame: UIScreen.main.bounds)
      window?.tintColor = R.color.purple()
      window?.backgroundColor = R.color.cloudBlueStart()
      return true
  }
  
  func applicationDidBecomeActive(_ application: UIApplication) {
      if window?.rootViewController == nil {
          window?.rootViewController = UIViewController()
          window?.makeKeyAndVisible()
      }
  }
  ```

  
