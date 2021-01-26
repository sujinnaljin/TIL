# LLDB
실시간으로 뷰 업데이트 하기
```
(lldb) settings set target.language swift
(lldb) e import UIKit
(lldb) e let $myObject = unsafeBitCast(0xabcdef, to: UIButton.self) // 0xabcdef에 할당된 UIButton 에 접근해 선언해주는것
(lldb) e $myObject.backgroundColor = .red
(lldb) e CATransaction.flush() //렌더 트리를 다시 그리고 시뮬레이터의 화면이 업데이트 함
```

## 출처
- [Xcode와 LLDB의 힘 : 실시간 애플리케이션 관리](https://ichi.pro/ko/post/110124929818574)

**그 외 참고 할만한 사이트**
- [[Xcode][LLDB]Debugging With Xcode and LLDB](http://minsone.github.io/ios/mac/xcode-lldb-debugging-with-xcode-and-lldb)
- [[Xcode][LLDB]Debugging With Xcode, LLDB and Chisel](http://minsone.github.io/ios/mac/xcode-lldb-debugging-with-xcode-lldb-and-chisel)
