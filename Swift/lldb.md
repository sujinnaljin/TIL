# LLDB (Low Level Debugger)
- LLVM 프론트엔드에 대응하는 디버거
- LLVM은 Apple 에서 진행한 Compiler에 필요한 Toolchain 개발 프로젝트로 컴포넌트들의 재사용성을 중시해서, 모듈화가 잘 되어있다는 특징이 있음. 이렇게 모듈화 되어있는 컴포넌트들을 이용해 진행된 주요 서브 프로젝트들로는 LLVM Core, Clang, libc++, LLDB 등이 존재
- LLDB는 LLVM의 Debugger Component를 개발하는 서브 프로젝트. LLVM 프로젝트를 통해 개발된 Clang Expression Parser, LLVM Diassembler 등 Low-Level 컨트롤이 가능한 모듈들로 이루어져 있어, 기계어에 가까운 영역까지 디버깅 가능하다는 장점이 있음
- C, C++, Objective-C, Swift를 지원하며, 현재 Xcode의 기본 디버거로 내장되어 있음.
## 실시간으로 뷰 업데이트 하기
```
(lldb) settings set target.language swift
(lldb) e import UIKit
(lldb) e let $myObject = unsafeBitCast(0xabcdef, to: UIButton.self) // 0xabcdef에 할당된 UIButton 에 접근해 선언해주는것
(lldb) e $myObject.backgroundColor = .red
(lldb) e CATransaction.flush() //렌더 트리를 다시 그리고 시뮬레이터의 화면이 업데이트 함
```

## 출처
- [Xcode와 LLDB의 힘 : 실시간 애플리케이션 관리](https://ichi.pro/ko/post/110124929818574)
- [LLDB(Low-Level Debugger)란?](https://yagom.net/courses/start-lldb/lessons/lldblow-level-debugger%eb%9e%80/)

**그 외 참고 할만한 사이트**
- [[Xcode][LLDB]Debugging With Xcode and LLDB](http://minsone.github.io/ios/mac/xcode-lldb-debugging-with-xcode-and-lldb)
- [[Xcode][LLDB]Debugging With Xcode, LLDB and Chisel](http://minsone.github.io/ios/mac/xcode-lldb-debugging-with-xcode-lldb-and-chisel)
