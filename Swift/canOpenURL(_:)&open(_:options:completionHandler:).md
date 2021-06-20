# canOpenURL(_:)과 open(_:options:completionHandler:)

## canOpenURL(_:)

- **URL scheme을 핸들링**할 수 있는지, 가능 여부 return 
- 앱이 깔려있지 않거나, 깔려 있더라도 Info.plist 의 `LSApplicationQueriesSchemes` 에 스카마가 정의되어 있지 않으면 `false` 반환
- `true`로 반환되면 `open(_:options:completionHandler:)` 메서드에 호출도 성공적으로 실행함을 보장
- 스키마에는 `http`, `https`, `tel`, `facetime`과 같은 공통 스키마 존재
- 참고로 custom URL scheme 대신 universal link를 사용하면, target link 에 대한 유효성을 확인할 필요가 없음. universal link를 처리할 수 있는 앱이 없으면 iOS가 이를 Safari로 라우팅하여 관련 웹 사이트가 응답. 

## open(_:options:completionHandler:) (Deprecated)

- URL에 대해 open 시도
- `LSApplicationQueriesSchemes` 정의된 것에 제약 받지 않음. 스키마가 정의됨 여부와 상관없이 app이 url을 handle 할 수 있으면 실행



# 출처

- [canOpenURL(_:)](https://developer.apple.com/documentation/uikit/uiapplication/1622952-canopenurl)
