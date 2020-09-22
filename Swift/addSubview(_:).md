# addSubview(_:)
receiver의 subview list 가장 마지막에 view를 add 한다
```swift
func addSubview(_ view: UIView)
```
## view
추가되고 난 후 다른 subview 보다 **가장 상위**에 나타난다
## Discussion
해당 메소드는 view에 대해 **강한 참조 (strong reference)** 를 걸고, view의 next responder를 receiver(새로운 슈퍼 뷰)에 설정한다.

addSubview(_:)의 파라미터 **view는 하나의 superview만 가질 수 있기** 때문에 **이미 superview가 있고 해당 superview가 receiver가 아니라면, 새로운 superview를 설정하기 전에 이전의 superview는 remove** 한다.

## 느낀점
위의 설명을 보고 기존에 설정된 superview와 새롭게 설정할 superview가 다를 때만 remove하는 줄 알았는데 아래의 코드를 두번 실행(같은 슈퍼뷰에 대해 addSubview(_:) 호출)시켜도 이전에 add되었던 뷰는 remove되고 새로운 frame에 다시 올려진다
```swift
self.view.addSubview(subView)
```
