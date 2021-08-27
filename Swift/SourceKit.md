# SourceKit

- **인덱싱, 구문 색상 지정, 코드 완성** 등과 같은 IDE 기능을 지원하기 위한 프레임워크

- 일반적으로 **SourceKit 위에 IDE 구축** 가능 -> **Xcode**가 하는 일

- 즉, Swift를 작성했거나 Xcode를 사용했다면 SourceKit과 여러 번 상호 작용 이미 함

- 다른 사람들이 SourceKit 위에 구축한 것의 예시는 아래와 같음

  - Jazzy - Swift용 문서 생성
  - SwiftLint - linter 
  - Swift Refactorator - 리팩토링 툴

- 풀 네임은 **SourceKit-LSP**인 듯. **Swift 및 C 기반** 언어용 **LSP** ( [Language Server Protocol](https://github.com/sujinnaljin/TIL/blob/master/LSP%20(Language%20Server%20Protocol).md) ) 의 구현

  > **LSP?**
  >
  > 프로그래밍 언어에 대해 **자동 완성**, **정의로 이동** 또는 **마우스를 가져갈 때 문서 보기**와 같은 기능을 추가하려면 상당한 노력이 필요
  >
  > 각 개발 툴은 **동일한 기능**을 구현하기 위해 **서로 다른 API를 제공**하므로 전통적으로 각 개발 도구에 대해 이 작업을 반복해야 했음
  >
  > Language Server는 **language-specific smarts를 제공** 하고, 프로세스 간 통신을 가능하게 하는 **프로토콜을 통해 개발 도구와 통신**
  >
  > LSP(Language Server Protocol)의 목적은 이러한 **서버 및 개발 도구의 통신 방식에 대한 프로토콜을 표준화**하는 것. 이렇게 하면 **단일 Language Server를 여러 개발 도구에 재사용**할 수 있으므로 최소한의 노력으로 여러 언어를 지원 가능
  >
  > LSP(Language Server Protocol)는 자동 완성, 정의로 이동, 모든 참조 찾기 등과 같은 언어 기능을 제공하는 **언어 서버 간와 편집기 (or IDE)간에  사용되는 프로토콜을 정의**
  >
  > Language Server Index Format(LSIF, "else if" 처럼 발음 됨)의 목표는 소스 코드의 로컬 복사본 없이 개발 도구 또는 웹 UI에서 풍부한 코드 탐색을 지원하는 것

- SourceKit-LSP 은 아래와 같은 기능 제공

  |                     |        |                                                              |
  | ------------------- | ------ | ------------------------------------------------------------ |
  | Feature             | Status | Notes                                                        |
  | Swift               | ✅      |                                                              |
  | C/C++/ObjC          | ✅      | Uses [clangd](https://clangd.llvm.org/)                      |
  | Code completion     | ✅      |                                                              |
  | Quick Help (Hover)  | ✅      |                                                              |
  | Diagnostics         | ✅      |                                                              |
  | Fix-its             | ✅      |                                                              |
  | Jump to Definition  | ✅      |                                                              |
  | Find References     | ✅      |                                                              |
  | Background Indexing | ❌      | Build project to update the index using [Indexing While Building](https://github.com/apple/sourcekit-lsp#indexing-while-building) |
  | Workspace Symbols   | ✅      |                                                              |
  | Global Rename       | ❌      |                                                              |
  | Local Refactoring   | ✅      |                                                              |
  | Formatting          | ❌      |                                                              |
  | Folding             | ✅      |                                                              |
  | Syntax Highlighting | ❌      | Not currently part of LSP.                                   |
  | Document Symbols    | ✅      |                                                              |



# 출처 

- [apple/sourcekit-lsp](https://github.com/apple/sourcekit-lsp)
- [SourceKit and You](https://academy.realm.io/posts/appbuilders-jp-simard-sourcekit/)

