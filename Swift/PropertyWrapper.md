# Property Wrapper
- 프로퍼티의 getter/setter에 들어갈 코드가 여러 프로퍼티에서 재사용되는 경우, 그것을 Property wrapper로 정의해놓으면 쉽게 그 동작을 끌어다 쓸수 있다. (프로퍼티의 저장부-getter/setter 동작이 정의된 코드-와 정의부-실제로 프로퍼티를 사용하는 코드-를 분리하고 싶을때도 유용)

- Property Wrapper를 정의하는 방법: 구조체, ENUM, 클래스 등을 만들고 @propertyWrapper라고 명시해주기. 실제 저장값은 wrappedValue로 정의, 추가로 제공하고 싶은 정보가 있다면 projectedValue를 정의할 것

- Property Wrapper를 밖에서 가져다쓰는 방법: 나의 프로퍼티를 정의하고 @프로퍼티래퍼명으로 attribute를 붙인다. 그러면 이 프로퍼티는 get/set동작이 이루어질때마다 알아서 Property wapper의 getter/setter 코드를 통해 실행됨


# 출처
- [Property Wrapper](https://wlaxhrl.tistory.com/90)