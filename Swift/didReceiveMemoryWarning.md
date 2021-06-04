# didReceiveMemoryWarning

- app이 메모리 warning을 받았을때 view controller에서 받게 되는 것
- system이 가능한 메모리가 적다(low)고 판단할때 해당 메소드가 호출 됨 (앱 내에서 절대 직접적으로 부르지 않음)
- view controller에서 사용하는 추가적인 메모리를 해제하기 위해 해당 메소드를 override 할 수 있음. 이때 메소드의 구현 어디선가에서는 반드시 super 호출해야함



# 출처 

- [didReceiveMemoryWarning](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621409-didreceivememorywarning?preferredLanguage=occ)

