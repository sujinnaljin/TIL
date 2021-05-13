# AssociatedObject

- 클래스 수정없이 새로운 값을 추가해야하는 경우가 있음. 이럴때 사용할수 있는것이 Associated Object (or Associated References).

  ex. extension을 사용할 경우에 값을 주입시켜서 사용하여야 하는 경우

- 즉, **런타임시 기존 클래스에 SubClassing 없이 사용자 속성이나 메소드들을 연결**(추가) 가능

- Association은 **key 기반**으로 동작. 서로 다른 키로 여러가지 연관 객체를 소스 객체에 등록 가능. 

- Association Reference는 소스 객체의 라이프 사이클 내에 연관 객체의 사용을 보장. 즉, 소스 객체가 해제되면 소스 객체에 추가된 연관 객체도 해제.

## AssociatedObject 관련 함수

```swift
// 객체에 속성 값을 연결 
public func objc_setAssociatedObject(_ object: Any,
                                     _ key: UnsafeRawPointer, 
                                     _ value: Any?, 
                                     _ policy: objc_AssociationPolicy) 

// 연결된 속성 값을 반환 
public func objc_getAssociatedObject(_ object: Any, 
                                     _ key: UnsafeRawPointer) -> Any? 

// 객체에 연결된 모든 속성 값을 제거 
// 제거하면 안되는 속성까지 제거될 수 있기 때문에, 
// 일반적으로 objc_setAssociatedObject을 nil로 설정하여 필요한 속성만 제거
public func objc_removeAssociatedObjects(_ object: Any)
```

**parameters**

- `object` - 속성을 연결하려는 객체

- `key` - 속성에 대한 키 값

- `value` - 객체와 연결하려는 속성 값

- `policy` - 속성 연결 시 참조 유형을 정의하는 값

  - OBJC_ASSOCIATION_ASSIGN (연관된 오브젝트에 대한 약한 참조를 지정)
  - OBJC_ASSOCIATION_COPY (연관된 오브젝트가 복사되고 ATOMIC으로 설정)
  - OBJC_ASSOCIATION_COPY_NONATOMIC (연관된 오브젝트가 복사되고 NONATOMIC으로 설정)
  - OBJC_ASSOCIATION_RETAIN (연관된 오브젝트에 대한 강력한 참조를 지정하고 ATOMIC으로 설정)
  - OBJC_ASSOCIATION_RETAIN_NONATOMIC (연관된 오브젝트에 대한 강력한 참조를 지정하고 NONATOMIC으로 설정)

  

## 사용 예시

```swift
extension UIButton {
    private struct AssociatedKeys {
        static var test = "Test" //값을 저장하기 위해 선언된 키
    }
    
    var test: String? {
        get {
            //키에 저장된 값을 불러옴
            return (objc_getAssociatedObject(self, &AssociatedKeys.test) as? String)
        }
        set {
            guard let newValue = newValue as String? else { return }
            //키에 값을 저장
            objc_setAssociatedObject(self, 
                                     &AssociatedKeys.test, 
                                     newValue, 
                                     .OBJC_ASSOCIATION_RETAIN_NONATOMIC)
        }
    }
}
 
```

# 출처

-  [iOS. Associated References로 임의 객체 저장](https://mrgamza.tistory.com/590)

-  [[swift] AssociatedObject](https://jintaewoo.tistory.com/52)

-  [[iOS] Associated References – 클래스 수정없이 임의 객체 저장](https://soulpark.wordpress.com/2012/10/10/ios-associated-references/)

