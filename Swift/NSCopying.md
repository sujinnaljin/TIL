# NSCopying
값을 변경해도 다른 object에 영향을 주지 않는 object 복사본을 만들고 싶을 때가 있다. 이를 위해선,,

- `NSCopying` 을 conform한다. 이는 엄격히 요구되는 사항이 아니지만, **의도를 명확히** 할 수 있다.
- 실제 복사가 일어나는  `copy(with:)` 를 Implement한다. 
- object에 대해 `copy()` 를 호출한다.
