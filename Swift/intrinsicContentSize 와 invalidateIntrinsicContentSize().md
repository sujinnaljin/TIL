# intrinsicContentSize 와 invalidateIntrinsicContentSize()

## intrinsicContentSize

- **본질적인 컨텐츠 크기**
- 대부분의 view들은 기본적으로 컨텐츠 크기만큼의 size를 가지고 있음
- UILabel이나 UIButton을 auto layout으로 구현할때 제약사항으로 width와 height를 추가하지 않아도 오류가 나지 않는 이유는 intrinsicContentSize를 가지고 있기 때문

## invalidateIntrinsicContentSize()

- view의 컨텐츠 크기가 바뀌었을때 **`intrinsicContentSize` 프로퍼티를 통해 size를 갱신**하고 그에 맞게 auto layout이 업데이트 되도록 만들어주는 메소드
- 즉 **view의 content 크기가 바뀔때 invalidateIntrinsicContentSize 를 호출하면 intrinsicContentSize 값이 새로 계산**되어 적용 됨
- apple에서 제공해주는 view는 내부적으로 적용되어 있음
- custom view를 구현할때는 intrinsicContentSize 프로퍼티와 더불어 invalidateIntrinsicContentSize() 메소드 구현 필요

```swift
public var point: CGFloat = 0 {
  didSet {
    self.currentWidth = getRateToWidth(self.current)
    self.maxWidth = getRateToWidth(CGFloat(self.max))
    
		// view의 컨텐츠 크기가 바뀔때 invalidateIntrinsicContentSize() 메소드를 실행
    invalidateIntrinsicContentSize()
  }
}

public override var intrinsicContentSize: CGSize {
  let count = CGFloat(self.max)
  var width = self.point * count
  width = width + CGFloat(count - 1) * self.spacing
  return CGSize(width: width, height: self.point)
}
```

# 출처

- [ios intrinsicContentSize에 대해서 알아보기](https://magi82.github.io/ios-intrinsicContentSize/)

