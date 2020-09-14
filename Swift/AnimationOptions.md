# AnimationOptions
- UIView의 AnimationOptions 에는 여러가지가 있다
## repeat
- **Animation을 반복**하는 옵션
- 다만 무한 반복이기 때문에 **completion에 들어가지 않는다**. 
- `view.layer.removeAllAnimations()` 등의 작업으로 애니메이션을 멈출 수 있다
## autoreverse
- **자동으로 애니메이션을 앞뒤로 실행**해주는 옵션
- 반드시 **repeat 옵션과 같이 사용**해야 한다

