# viewWillAppear 동작 context
viewWillAppear는 foreground에서 실행 중인 app에 대해 동작한다. 다른 앱에 갔다가 다시 돌아와서 foreground 로 보일때는 동작하지 않음. 따라서 `UIApplication.willEnterForegroundNotification` 등록 필요

```swift
override func viewDidLoad() {
  super.viewDidLoad()

  // set observer for UIApplication.willEnterForegroundNotification
  NotificationCenter.default.addObserver(self, 
                                         selector: #selector(willEnterForeground), 
                                         name: UIApplication.willEnterForegroundNotification, 
                                         object: nil)
}

@objc func willEnterForeground() {
}

deinit {
  NotificationCenter.default.removeObserver(self,
                                            name: UIApplication.willEnterForegroundNotification, 
                                            object: nil)
}
```

# 출처
- [Why does viewWillAppear not get called when an app comes back from the background?](https://stackoverflow.com/questions/5277940/why-does-viewwillappear-not-get-called-when-an-app-comes-back-from-the-backgroun)


