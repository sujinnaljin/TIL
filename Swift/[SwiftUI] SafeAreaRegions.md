# [SwiftUI] SafeAreaRegions

`.ignoresSafeArea(.all, edges: .top)` 에서 첫번째로 들어가는 regions 인자의 타입

![image](https://user-images.githubusercontent.com/20410193/172457459-3839e143-2e80-4658-aa80-7ae3f69cf10d.png)



## 기본

safe area 지켜짐

```
    var body: some View {
        ZStack {
            Color.pink
            VStack {
                Spacer()
                TextField("I am behind the keyboard 👀", text: $text)
            }
        }
    }
```

<img width="518" alt="image" src="https://user-images.githubusercontent.com/20410193/172457497-f21b419c-7ed3-437b-9ed1-4e2d86627199.png">

## all

스태이터스 바, 키보드 safe area 모두 무시

```
    var body: some View {
        ZStack {
            Color.pink
            VStack {
                Spacer()
                TextField("I am behind the keyboard 👀", text: $text)
            }
        }
        .ignoresSafeArea(.all, edges: [.top, .bottom])
    }
```
<img width="512" alt="image" src="https://user-images.githubusercontent.com/20410193/172457929-a64d3c33-fd88-4901-8795-1bb64c4fee74.png">


## keyboard

스태이터스 바 safe area 는 지키되, 키보드는 무시

```
    var body: some View {
        ZStack {
            Color.pink
            VStack {
                Spacer()
                TextField("I am behind the keyboard 👀", text: $text)
            }
        }
        .ignoresSafeArea(.keyboard, edges: [.top, .bottom])
    }
```
<img width="502" alt="image" src="https://user-images.githubusercontent.com/20410193/172457941-04be18f4-f12d-4f99-917e-ce625d34e6ba.png">


## container

스태이터스 바 safe area 는 무시, 키보드는 지킴

```
    var body: some View {
        ZStack {
            Color.pink
            VStack {
                Spacer()
                TextField("I am behind the keyboard 👀", text: $text)
            }
        }
        .ignoresSafeArea(.container, edges: [.top, .bottom])
    }
```

<img width="495" alt="image" src="https://user-images.githubusercontent.com/20410193/172457962-c3693e6c-9b8b-48b5-8385-2afc95e032ab.png">

# 출처

- https://developer.apple.com/documentation/swiftui/path/ignoressafearea(_:edges:)

- https://swiftontap.com/safearearegions
