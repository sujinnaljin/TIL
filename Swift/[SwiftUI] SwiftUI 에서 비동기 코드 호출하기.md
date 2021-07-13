# [SwiftUI] SwiftUI 에서 비동기 코드 호출하기

- 비동기 작업에는 아래와 같은 것들이 있음
  - 디스크에서 파일 읽기
  - 웹의 원격 API에서 데이터 가져오기
  - 사용자로부터 입력 받기
- 비동기 작업을 호출 할 수 있는 곳들은 다양함

## 뷰가 나타날 때

- 가장 일반적으로 데이터를 가져오는 곳

- 동기적인 context에서 비동기 task를 시작하기 위해서는 `async` 를 사용

  ```swift
      .onAppear {
        async {
          await viewModel.executeQuery(for: word)
        }
      }
  ```

- `onAppear` 외에 더 나은 다른 방법도 존재. view가 appear될때 데이터를 가져오는 상황은 일반적이기 때문에 자동으로 `Task` 를 생성하고 view가 disappear 되면 cancel하는 새로운 view modifier를 만듦

  ```swift
      .task {
        await viewModel.executeQuery(for: word)
      }
  ```

## 버튼을 탭할 때

- 대부분의 button action 핸들러는 비동기 코드 호출을 지원하지 않기 때문에 `async` 를 호출해서 새로운 비동기 컨텍스트를 만들어줘야함

```swift
    Button("Refresh") {
      async {
        await viewModel.refresh()
      }
    }
```

## 새로고침 할 때

- 비동기 컨텍스트여서 async 안써도 됨

```swift
      .refreshable {
        await viewModel.refresh()
      }
```

## 검색 요청에 대한 응답으로

- `onSubmit`의 클로저는 비동기식이 아니기 때문에  `async { }` 구문을 사용하여 Task 를 새로 만들어야 함

 ```swift
      .onSubmit(of: .search) {
        async {
          await viewModel.executeQuery()
        }
      }
 ```



# 출처

- [Getting Started With Async/Await in SwiftUI for iOS 15](https://betterprogramming.pub/getting-started-with-async-await-in-swiftui-for-ios-15-f627eb722a4b)

