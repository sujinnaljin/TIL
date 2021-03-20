# UIView의 생성자 (feat. `init(frame:)` 을 지정할 때 `required init?(coder: NSCoder)` 도 같이 생성해야하는 이유 )

UIView의 Designated Initializer(class를 위한 필수 initializer) 은 아래 두개.

## init(frame:)

- **코드로** 뷰를 만들때 호출. 
- () 로 생성하면 내부적으로 init(frame:) 호출.

## init?(coder:)

- **Interface Builder** 에서 나오면서 UIView를 만들때 만들어지는 생성자
- @IBOutlet 으로 연결하거나 loadFromNib 같은 걸로 생성하면 해당 함수 호출
- 이 때 init(coder:)가 호출되며 내부 속성이 초기화 될때 **frame과 관련된 크기, 위치 등이 정해지지 않은** 상태
- IBOutlet 같이 연결된 객체의 변수를 사용하려고 할 때 `awakeFromNib()` 에서 사용. 왜냐면 해당 생성자는 IBOutlet 과 같은 처리를 하는 시점이므로 해당 참고 객체는 null

## CustomView에서 `init(frame:)` 을 지정할 때 `required init?(coder: NSCoder)` 도 같이 생성해야하는 이유 

- Swift는 기본적으로 서브클래스가 수퍼클래스의 이니셜라이저들을 상속받지 않은 것을 원칙으로 하고 특별한 경우에 자동 상속을 허용한다. 이니셜라이저의 자동상속은 다음의 조건을 만족해야 한다.
  1. 만약 자식 클래스에서 추가된 저장 프로퍼티가 모두 디폴트 값을 가지고 있고, 추가적인 지정 이니셜라이저를 작성하지 않았다면(여기서는 추가되는 지정이니셜라저 혹은 지정이니셜라이저의 오버라이드가 모두 포함된다.) 부모 클래스의 모든 지정 이니셜라이저를 상속받는다.
  2. 부모의 이니셜라이저를 상속받았거나, 아니면 부모의 모든 지정 이니셜라이저를 오버라이드했다면 부모의 편의 이니셜라이저를 자동으로 상속받는다.

여기서 문제 상황은 2번이다. `init(frame:)` 을 오버라이드했다는 것은 부모의 일부 지정 이니셜라이저를 오버라이드했다는 것이다. 이 경우에는 이니셜라이저 상속을 포기한 것이 되고, `init(coder:)` 는 자동으로 상속되지 않기 때문에 수동으로 직접 작성해야 한다. 즉 **일부 이니셜라이저만 override 하면 안되기 때문**





# 출처

- [커스텀 뷰 클래스에 속성 지정하기, UIView의 생성자 - init(), awakeFromNib()](https://jinios.github.io/ios/2018/04/15/customView_init/)

- [[iOS - swift] init(frame:), required init?(coder aDecoder: NSCoder), prepareForInterfaceBuilder(), awakeFromNib() 초기화의 정체](https://ios-development.tistory.com/222)
- [required init?(coder: NSCoder) 에 대해서](https://medium.com/@b9d9/required-init-coder-nscoder-%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-b67ddfc628)

- [required init()과 override init()의 차이는 무엇일까](https://sweetdev.tistory.com/366)



