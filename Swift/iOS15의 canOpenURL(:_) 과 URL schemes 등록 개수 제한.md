# iOS15의 canOpenURL(:_) 과 URL schemes 등록 개수 제한 

-  `canOpenURL(_:)` 로 전달하는 URL scheme 에 대해서는 Info.plist 의 `LSApplicationQueriesSchemes` 에 등록을 해야함. 

- 만약 등록이 안되어있으면 앱이 깔려있든 말든 `canOpenURL(_:)` 은 false 를 반환

- 근데 iOS 15 부터는  `LSApplicationQueriesSchemes` 에 등록할 수 있는 개수가 최대 50개로 제한 됨

- 해당 메소드와 달리 [`openURL:options:completionHandler:`](https://developer.apple.com/documentation/uikit/uiapplication/1648685-openurl?language=objc) 은  `LSApplicationQueriesSchemes` 에 제약을 받지 않음

  만약 앱이 URL을 처리할 수 있는 경우, 시스템은 사용자가 scheme 선언했는지 여부에 관계없이 해당 URL을 실행

- custom URL schemes 대신 universal link를 사용하면 타겟 링크의 유효성을 확인하기 위해 canOpenURL 을 사용할 필요가 없음. 만약 universal link를 처리할 수 있는 앱이 없는 경우, iOS는 연결된 웹 사이트가 응답할 수 있도록 Safari 로 이 링크를 라우팅



# 출처

- [LSApplicationQueriesSchemes Xcode 13 or iOS15 BUG?](https://stackoverflow.com/questions/69370895/lsapplicationqueriesschemes-xcode-13-or-ios15-bug)
- [canOpenURL(_:)](https://developer.apple.com/documentation/uikit/uiapplication/1622952-canopenurl)
