# Enum cases as protocol witnesses
1. 기존 protocol에서 static을 선언하고 enum에서 conform하면 똑같이 static을 구현해야했다.
2. 하지만 static과 enum이 동작하는 방식은 같다. 
3. 따라서 이넘에서 구현해야하는 static 을 case로 대체할 수 있도록 한다
4. Case로 대체될 수 있는 protocol의 프로퍼티나 메소드의 특성은 "스태틱 & Self나 enum타입 리턴" 이어야한다

# 참고
- [Swift 5.3 released!](https://medium.com/official-podo/swift-5-3-released-83ab5a7c07f6)
- [Enum cases as protocol witnesses](https://github.com/apple/swift-evolution/blob/master/proposals/0280-enum-cases-as-protocol-witnesses.md)
