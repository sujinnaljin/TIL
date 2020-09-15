# AnimationOptions
- UIView의 AnimationOptions 에는 여러가지가 있다
## repeat
- **Animation을 반복**하는 옵션
- 다만 무한 반복이기 때문에 **completion에 들어가지 않는다**. 
- `view.layer.removeAllAnimations()` 등의 작업으로 애니메이션을 멈출 수 있다
- 근데 ViewWillAppear에서 호출하면 바로 completionBlock이 실행되는 걸 보니 타이밍 이슈도 있는듯
  
  **Autolayout constraints은 viewWillAppear에서 만족되지 않는다**. **viewWillAppear -> viewWillLayoutSubviews -> viewDidLayoutSubviews이 constraint를 만족하기 위해 차례대로** 불리고 그 다음으로 **constraint가 잘 세팅 된 viewDidAppear**가 호출된다. 그 다음에 animation을 수행해야한다
## autoreverse
- **자동으로 애니메이션을 앞뒤로 실행**해주는 옵션
- 반드시 **repeat 옵션과 같이 사용**해야 한다

