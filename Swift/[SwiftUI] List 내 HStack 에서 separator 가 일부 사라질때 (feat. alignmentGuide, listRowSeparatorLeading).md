# [SwiftUI] List 내 HStack 에서 separator 가 일부 사라질때 (feat. alignmentGuide, listRowSeparatorLeading)

List 에서 HStack 으로 Shape 와 Text 를 결합하면 Shape width 만큼 separator가 사라졌음 

```swift
List {
                HStack {
                    Circle()
                        .fill(.gray)
                        .frame(width: 60, height: 60)
                        .overlay {
                            Text(String(contact.name.first!))
                                .font(.title3)
                                .foregroundStyle(.white)
                        }
                    Text("Contact Photo")
                        .font(.title3)
                }
}
```

<img width="791" alt="image" src="https://github.com/sujinnaljin/TIL/assets/20410193/1134a922-dd65-40e3-9c26-772b6e09dcb7">


Shape + Image 조합은 괜찮았음

<img width="788" alt="image" src="https://github.com/sujinnaljin/TIL/assets/20410193/b101ab96-cee0-4934-b26f-f6b2ddf458e0">

-----

iOS 16에서는 List 및 Form의 행 구분자가 자동으로 간격을 조절하며 텍스트와 정렬되게 되었음. 또한, 두 가지 새로운 정렬 속성인 [`listRowSeparatorLeading`](https://developer.apple.com/documentation/swiftui/horizontalalignment/listrowseparatorleading) 및 [`listRowSeparatorTrailing`](https://developer.apple.com/documentation/swiftui/horizontalalignment/listrowseparatortrailing)이 도입되었으며, 이는 [`alignmentGuide(_:computeValue:)`](https://developer.apple.com/documentation/swiftui/view/alignmentguide(_:computevalue:)-6y3u2)와 함께 사용할 수 있음.

```swift
            List {
                HStack {
                    Circle()
                        .fill(.gray)
                        .frame(width: 60, height: 60)
                        .overlay {
                            Text(String(contact.name.first!))
                                .font(.title3)
                                .foregroundStyle(.white)
                        }
                    Text("Contact Photo")
                        .font(.title3)
                }
                // 여기 추가
                .alignmentGuide(.listRowSeparatorLeading) { viewDimensions in
                    return 0
                }
```
<img width="764" alt="image" src="https://github.com/sujinnaljin/TIL/assets/20410193/f4f7ffe5-8489-4b75-bdce-5d7dea2fb433">


참고로 listRowSeparatorTrailing 으로 하면 아래처럼 나옴

<img width="800" alt="image" src="https://github.com/sujinnaljin/TIL/assets/20410193/ba7462e5-6277-4b64-9da6-1a63b4fc3976">

## 참고

- https://stackoverflow.com/questions/74845212/how-do-we-control-the-automatic-dividers-in-swiftui-forms
- https://developer.apple.com/documentation/swiftui/view/alignmentguide(_:computevalue:)-9mdoh
