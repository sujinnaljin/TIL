# 웹뷰와 자바스크립트 연동 (Native <-> JavaScript 통신 방법)

- **Native -> 일반적인 JS함수 호출**

  1. WKWebView 의 `evaluateJavaScript(_:completionHandler:)` 함수 사용

  ```swift
  webView.evaluateJavaScript("getLocal()", completionHandler: {(result, error) in
      if let result = result {
          print(result)
      }
  })
  ```

- **Js -> Native 호출**

  1. `WKUserContentController`의 **`add(_:name:)`** 로  **JavaScript 코드에서 호출할 수 있는 메시지 핸들러를 추가**. 

     js 코드에서는 `webkit.messageHandlers.NAME.postMessage("");` 와 연동되는 것.

  ```swift
  let configuration = WKWebViewConfiguration()
  contentController.add(self, name: "callbackHandler")
  ```

  2. `WKScriptMessageHandler`의 **`userContentController(_:didReceive:)`** 에서 **웹 페이지가 스크립트 메시지를 보냈음**을 핸들러에게 알릴때 핸들링 코드 추가.

     `message.name`  및 `message.body` 를 검사하여 네이티브 함수를 호출 할 수 있음

  ```swift
         @available(iOS 8.0, *)
         func userContentController(_ userContentController: WKUserContentController, didReceive message: WKScriptMessage){
             if(message.name == "callbackHandler"){
                 if(message.body as! String == "abc"){
                     abc()
                 }
             }
         }
  ```

- 참고로 js에서 들어오는 **`window.open()`** 을 처리하기 위해서는 `WKUIDelegate` 의`webView:createWebViewWithConfiguration:forNavigationAction:windowFeatures: ` 에서 새창을 띄우면 된다

```swift
func webView(_ webView: WKWebView, createWebViewWith configuration: WKWebViewConfiguration, for navigationAction: WKNavigationAction, windowFeatures: WKWindowFeatures) -> WKWebView? {}
```

# 출처

- [[SWIFT] 웹뷰와 자바스크립트 연동 (Native <-> JavaScript 통신 방법)](https://g-y-e-o-m.tistory.com/13)

- [WKWebView Bridge 연결하기](https://swieeft.github.io/2020/07/29/WKWebViewBridge.html)

- [[iOS\]WKWebView를 이용한 하이브리드 앱(Hybrid App) 제작하기](https://jingyu.tistory.com/2)

- [WKWebView and window.open](https://stackoverflow.com/questions/33190234/wkwebview-and-window-open)

