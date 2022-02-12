# LSP (Language Server Protocol)

- 프로그래밍 언어에 대해 **자동 완성**, **정의로 이동** 또는 **마우스를 가져갈 때 문서 보기**와 같은 기능을 추가하려면 상당한 노력이 필요
- 각 개발 툴은 이러한 **동일한 기능**을 구현하기 위해 **서로 다른 API를 제공**하므로 전통적으로 각 개발 도구에 대해 이 작업을 반복해야 했음
- **Language Server**는 **구문 트리**를 구성해서 **텍스트 에디터에 API 로 제공**하는 소프트웨어. 프로세스 간 통신을 가능하게 하는 **프로토콜을 통해 개발 도구와 통신**. 
  
  다양한 텍스트 에디터를 지원할 수 있고, **정교한 코드 분석과 리팩터링** 기능을 제공할 수 있음. **language-specific smarts**를 제공.
- LSP(Language Server Protocol)의 목적은 이러한 **서버 및 개발 도구의 통신 방식에 대한 프로토콜을 표준화**하는 것. 이렇게 하면 **단일 Language Server를 여러 개발 도구에 재사용**할 수 있으므로 최소한의 노력으로 여러 언어를 지원 가능
- LSP(Language Server Protocol)는 자동 완성, 정의로 이동, 모든 참조 찾기 등과 같은 언어 기능을 제공하는 **Language Server와 편집기 (or IDE)간에  사용되는 프로토콜을 정의**
- Language Server Index Format(LSIF, "else if" 처럼 발음 됨)의 목표는 소스 코드의 로컬 복사본 없이 개발 도구 또는 웹 UI에서 풍부한 코드 탐색을 지원하는 것
- Swift의 SourceKit은 (SourceKit-LSP) **Swift 및 C 기반** 언어용 **LSP** 의 구현

# 출처

- [Language Server Protocol](https://microsoft.github.io/language-server-protocol/)

