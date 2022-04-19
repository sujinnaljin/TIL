# StackView 의 backgroundColor 적용

```
customStackView.backgroundColor = .blue
```

- UIStackView 는 non-drawing view 이기 때문에 drawRect() 가 호출되지 않고 background color 도 무시됨. 따라서 백그라운드 색상 바꾸고 싶으면 위의 코드는 동작하지 않고, 따로 stack view 에 UIView 넣어야함
- 하지만 iOS 14 이상부터 UIStackView의 layer 가 CATransformLayer에서 CALayer로 변경 되어 위의 코드 작동

# 출처

- [UIStackView에 Background Color 적용하기](https://velog.io/@dvhuni/UIStackView%EC%97%90-Background-Color-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0)

