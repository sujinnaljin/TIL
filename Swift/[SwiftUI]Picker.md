# [SwiftUI] Picker

- Picker를 생성할때 @Binding 프로퍼티를 넣어줘야하는데 도대체 어디에서 얘를 변경시켜주는건지 파악이 안됐음

```swift 
struct ContentView: View {
    var colors = ["Red", "Green", "Blue", "Tartan"]
    @State var selectedColor = 0
    
    var body: some View {
       Picker(selection: $selectedColor, label: Text("Please choose a color")) {
           ForEach(0 ..< colors.count) {
               Text(self.colors[$0])
           }
       }
    }
}
```

- 사실 이렇게 **.tag를 붙여서 @state variable이랑 연관**시켜주는거였음

```swift
@State private var selectedFlavor = Flavor.chocolate

Picker("Flavor", selection: $selectedFlavor) {
    Text("Chocolate").tag(Flavor.chocolate)
    Text("Vanilla").tag(Flavor.vanilla)
    Text("Strawberry").tag(Flavor.strawberry)
}
Text("Selected flavor: \(selectedFlavor.rawValue)")

```

### Iterating Over a Picker’s Options

이렇게 일일이 설정해주는 대신 [`ForEach`](https://developer.apple.com/documentation/swiftui/foreach) 로 생성을 하면 옵션의 `id`를 이용해서 자동으로 tag를 어사인해줌. (여기선 `Flavor`가 Identifiable protocol을 따르고 있기 때문에 가능)

```swift
Picker("Flavor", selection: $selectedFlavor) {    
  ForEach(Flavor.allCases) { flavor in      
    Text(flavor.rawValue.capitalized)    
  }
}
```



# 출처

- [SwiftUI Picker 사용하기](https://www.hohyeonmoon.com/blog/swiftui-tutorial-picker/)
- [Picker](https://developer.apple.com/documentation/swiftui/picker)

