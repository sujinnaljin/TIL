# Copy-On-Assignment vs. Copy-On-Write

## Copy-On-Assignment

- **struct** 객체를 만들고 그것을 **다른 객체에 할당**할 때 기본적으로 수행됨
- 즉, 객체를 다른 객체에 할당할 때 원래 객체의 **전체 복사**본이 생성되고 해당 컨텐츠뿐 아니라 **객체 내에서 참조되는 원격 리소스**도 복사
- 객체를 완전히 복사하는 것은 비용이 많이 든다고 생각할 수 있지만, **내부에 reference type이 없는 struct** 객체는 참조 카운트 오버헤드 없이 **완전히 스택에 할당**되므로 비용이 많이 들지 않음

## Copy-On-Write

- 내부에 **reference type이 있는 struct** 객체는 **힙을 가리키는 참조**가 있기 때문에 **전체 스택 할당으로 간주되지 않음**
- **힙**은 **공유**되기 때문에 **thread safety**가 필요하고, **할당 속도도 느리**기 때문에 해당 객체를 복사하는 데는 **많은 비용** 필요
- Copy-on-write는 표준 라이브러리의 **Array, Dictionary 및 Set**과 같은 모든 **가변 크기 컬렉션**에 추가되는 **최적화** 방법
- 복사본 중 하나를 **수정할 때까지 동일한 스토리지**를 공유. 즉, 복사본 중 하나가 **수정될 때까지 참조**가 만들어짐
- 속성 **수정 시 reference count를 확인** 후, **고유하게 참조(uniquely referenced) 되지 않으면 새 객체**를 만드는 식으로 동작

# 출처

- [Copy-On-Assignment vs. Copy-On-Write in Swift](https://aymanmoo.medium.com/copy-on-assignment-vs-copy-on-write-in-swift-c3016b343d06)

