# didReceiveMemoryWarning

- UIViewController의 instance 메소드로, app이 메모리 warning을 받았을때 view controller에서 받게 되는 것
- system이 가능한 메모리가 적다(low)고 판단할때 해당 메소드가 호출 됨 (앱 내에서 절대 직접적으로 부르지 않음)
- view controller에서 사용하는 추가적인 메모리를 해제하기 위해 해당 메소드를 override 할 수 있음. 이때 메소드의 구현 어디선가에서는 반드시 super 호출해야함
- 원래 iOS 6 이전에는 메모리 부족 이벤트가 발생했을 때 해당 뷰컨트롤러의 뷰가 필요하지 않다고 판단되면 시스템이 이를 임의로 메모리에서 해제할 수 있었음. 이 때 사용되던 메소드들이 `viewWillUnload` / `viewDidUnload` 지만, iOS6 이후로는 시스템이 이를 수행하지 않기 때문에 해당 메소드들은 deprecated 되고 대신 시스템 메모리가 부족한 상황이 되면 didReceiveMemoryWarning() 메소드 호출


# 출처 

- [didReceiveMemoryWarning](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621409-didreceivememorywarning?preferredLanguage=occ)
- [iOS 뷰컨트롤러의 생명주기](https://jcsoohwancho.github.io/2019-09-21-iOS-%EB%B7%B0%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC%EC%9D%98-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0/)

