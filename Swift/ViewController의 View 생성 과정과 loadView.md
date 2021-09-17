# ViewController의 View 생성 과정과 loadView

1. viewController.view등의 UIViewController 객체의 view property에 접근

2. 메모리상에 view가 존재하지 않으면, loadView를 호출. (이 메소드를 통해  view를 로드하거나 create해서 view property 에 할당)

3. loadView가 override가 되어 있으면, 그 안의 내용을 생성

4. loadView가 override가 안되어 있다면,  관련된 nibfile로 부터 view의 load를 시도. 

   > 관련 nib file 이 있다는건?
   >
   > - viewcontroller가 storyboard에서 초기화되어서 nibName property가 non-nil 값이거나, 
   > - init(nibName:bundle:) method 를 이용해서 nib file 을 명시적으로 할당했거나 
   > - iOS가 app bundle 에서 vc의 class 이름으로 된 nib file을 찾았다는 것. 

5. 만약 vc 관련 nib file 조차 없으면 해당 method는 plain UIView object를 생성

6. 이후 viewDidLoad가 호출됨



## loadView

- 해당 메소드를 직접적으로 호출해서는 안됨
- view controller는 이 메소드를 view property 가 요청 되지만 현재 nil 일때 호출함. 
- 만약 view나 vc 를 만들기 위해 Interface Builder를 사용했다면 해당 메소드를 override 하지 말아야함.
- 이건 view를 수동으로 생성할때 override 하는 것. 이 안에서  super는 호출 금지하고, `self.view = UIView()` 처럼 root view를 view property에 할당 해라.
- 그렇게 하기로 했다면, root view를 view property에 할당해라. 너가 만든 뷰들은 unique 해야하고 다른 vc 와 공유되어서는 안됨. 
- 만약 view에 대한 추가적인 초기화를 원하면 viewDidLoad 메소드 사용

# 출처

- [loadView()](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621454-loadview)

- [[iOS\]View가 Load 될 경우](https://mrgamza.tistory.com/279)

- [loadView()에 대한 간단한 고찰](https://learn-hyeoni.tistory.com/56)

