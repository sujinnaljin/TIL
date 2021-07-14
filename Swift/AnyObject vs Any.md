# AnyObject vs Any

- 이상적으로는 `Any` 나 `AnyObject` 의 사용을 지양하고 자세한 type 을 써주는게 더 나음

## AnyObject

- class의 any instance를 뜻함. 
- Object-C의 `id` 와 동일 
- 특히 reference type과 관련된 작업에서 유용. 왜냐하면 struct나 enum은 허용하지 않기 때문. 
-  `AnyObject` 는 class 한정으로 protocol을 제한하고 싶을때 사용되기도 함

## Any

- class, struct, 혹은 enum의 any instance를 뜻함 (말그대로 anything at all) 
- type을 모르거나, type이 mix 되어 있을 때 사용

```swift
let values: [Any] = [1, 2, "Fish"]
```

