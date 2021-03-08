# LLVM (feat. 스위프트의 빌드 단계 / 컴파일러 과정)

LLVM 은 오픈소스 **컴파일러 프로젝트** 또는 툴체인의 이름. (약자 아님)

![image](https://user-images.githubusercontent.com/20410193/110127199-b10a1c00-7e08-11eb-99ee-30f85ce63246.png)

보통 [컴파일러](https://namu.wiki/w/컴파일러)는 **프론트엔드-미들엔드-백엔드의 단계로 구성**되어 있다. 보통 이 세 단계는 하나의 프로그램으로 **일괄 처리**되는데, 이러한 구조는 **재사용성**을 떨어뜨린다는 문제가 있다. 이를 해결할 수 있는 컴파일러 구조가 **LLVM**이다.

**LLVM**은 아키텍처별로 분리된 모듈식 **미들엔드-백엔드를 중점**으로 하고 있다. 

1. **프론트엔드**가 여러가지 [프로그래밍 언어들을 **중간 표현 코드**로 번역하고,
2. **LLVM**은 그 중간 표현 코드를 각각의 아키텍처에 맞게 **최적화**하여 **실행이 가능한 형태**로 바꾸는 방식이다. 

처음에는 [GCC](https://namu.wiki/w/GCC)를 프론트엔드로 하고 LLVM은 미들엔드-백엔드로 사용하는 LLVM-GCC 프로젝트(DragonEgg)가 있었으나, **LLVM의 자체 프론트엔드인 Clang이 등장**한 이후 **컴파일의 전 과정을 LLVM 툴체인으로 진행**할 수 있게 되었다.

따라서 전체적인 **LLVM의 빌드 구조**는 아래와 같다.

`Frontend`->`LLVM Optimizer`->`LLVM Backend`


1. **Frontend** : C, Fotran, GO등의 언어를 **LLVM이 이해할 수 있는 중간 언어로 번역**하는 단계
2. **LLVM Optimizer** : 바이너리로 변환하기 전에 **최적화를 수행**
3. **Backend** : x86, ARM등의 CPU 아키텍쳐에 최적화된 **바이너리를 생성**



## LLVM 프로젝트

### 1. LLVM Core

LLVM **미들엔드/백엔드**를 구성하는 라이브러리. 

### 2. Clang (클랭)

C, C++, Objective-C 용 **컴파일러**. LLVM 프로젝트의 메인 **프론트엔드**로, 소스 코드를 **LLVM IR로 컴파일**하는 역할을 담당

### 2-1. Swift-Clang

**Swift**에 대응하는 **프론트엔드 컴파일러**. 애플이 기존의 LLVM을 포크해서 만든 **Swift-LLVM과 결합되어 돌아**간다. 기본적인 구조는 Clang과 유사하지만 스위프트 **소스 코드와 LLVM IR 사이**에 **SIL**(Swift Intermediate Language)이라는 또다른 **중간 코드**가 하나 더 있다. 즉 프론트엔드에서 스위프트 코드는 SIL로 컴파일될 때 한 번, LLVM IR로 컴파일될 때 한 번 해서 총 **두 번 최적화**되는 것이다.

![img](https://blog.kakaocdn.net/dn/ZbUmK/btqO6f8YlZo/kWMuzfKIJeXKLnwNOok4k1/img.jpg)

### cf) LLDB

- LLVM 프론트엔드에 대응하는 디버거



## 스위프트의 빌드 단계 / 컴파일러 과정

따라서 Swift 소스 코드로 바이너리 코드를 생성할 때, 컴파일러는 다음의 과정을 거친다.

`Swift Code`  → `구문분석(AST 생성)` → `의미분석 (타입 정보 포함한 AST 생성)` → `모듈 임포트` → `SIL 생성 (raw SIL)` → `SIL 정규화 (canonical SIL)` → `SIL 최적화` → `LLVM IR 생성` (여기까지 LLVM의 FrontEnd) → `최적화` → `Compiler backend` → `Assembler` → `링커`

- **Swift AST**(Abstract Syntax Tree) 

  - 스위프트의 **문법 분석**(예약어 검사, 구현 등을 제외한 순수한 구문 분석)을 수행.

- **Swift SIL** (Swift Intermedate Language)

  - Swift 코드와 LLVM IR과의 중간에 위치 (`AST` - `SIL` - `LLVM IR`)
  - Swift 소스 코드와 **LLVM IR과의 표현의 차이를 메꾸**기 위함
  - `Raw Swift IL`과 `Canonical Swift IL` 두 형태 존재. 
  - LLVM IR이 다루기 힘든 **Swift 소스 코드 레벨**에서의 **정적 분석**도 범위에 들어감. (코드 하이라이팅이나, auto completion 등의 기능도 포함)

  - 여기에서 스위프트, AST에서 나타나는 규칙적인 패턴과 각 문법의 구분이 흐려지고, 함수, 클로져, 변수등은 모두 동등한 구성으로 재배치 됨. 여기까지가 **LLVM**에서의 **Frontend**

- **LLVM IR** (Intermedate Representation)

  - LLVM IR은 아래와 같이 분류 됨

    1. LLVM 어셈블리 언어(LLVM assembly language) -> `.ll` 확장자인 텍스트 파일로 저장. 사람이 읽을 수 있는 문자열로 표현됨.

    2. LLVM 비트코드(bitcode) -> `.bc` 확장자인 바이너리 파일로 저장. 바이너리로 표현 되는, 아직 기계코드도 아니고 내가 이해 할 수 있는 코드도 아닌 중간단계의 코드. 

       LLVM 어셈블리 언어와 LLVM 비트코드는 표현 형식만 다르고, 서로 간에 자유롭게 전환할 수 있다. LVM 명령어 도구인 llvm-dis를 사용해 LLVM 비트코드를 LLVM 어셈블리 언어로 전환할 수 있으며, 반대로 llvm-as를 사용해서 LLVM 어셈블리 언어를 LLVM 비트코드로 전환할 수 있다.

       `ex. test.ll → llvm-as → test.bc`

    3. C++ 목적 코드(C++ Object Code) -> `.o` 확장자

- **최적화(optimizer)**

  - LLVM 패스(pass)라는 단위로 관리
  - 각각의 최적화 패스는 전달받은 LLVM IR을 최적화한 뒤, 최적화한 LLVM IR을 그다음 패스로 전달

- **Compiler Backend**

  - 최적화된 LLVM IR을 특정 CPU 아키텍처에 의존적인 어셈블리(assembly) 언어로 변환

- **Assembler**

  - 어셈블리 언어를 기계어로 변경해 확장자가 `.o`인 오브젝트(object) 파일로 저장

- **링커**

  - 참조 관계 확인 및 저장된 오브젝트 파일들을 하나로 묶어서 실행(executable) 파일 또는 공유 라이브러리(shared object) 파일을 생성
  - 이 단계에서 링크 시점 최적화(LTO, Link Time Optimizer)도 수행될 수 있음







# 출처

- [SIL(Swift Intermediate Language)을 통한 Swift debugging](https://woowabros.github.io/swift/2018/03/18/swift-debugging-with-SIL.html)
- [LLVM](https://namu.wiki/w/LLVM)
- [[번역] SIL(Swift Intermediate Language), 일단 시작해보기까지](https://woowabros.github.io/swift/2018/03/18/translation-SIL-for-the-moment-before-entry.html)
- [LLVM이란](https://zeddios.tistory.com/1175)
- [Swift Compiler](https://swift.org/swift-compiler/#compiler-architecture)
- [오크(ORK) – 난독화 컴파일러 도구 1편](https://engineering.linecorp.com/ko/blog/code-obfuscation-compiler-tool-ork-1/)

