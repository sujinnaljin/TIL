

# Swift 컴파일 실행 프로세스 (feat. Driver, Frontend Jobs)

## 배경

- Xcode나 command line 에서 Swift 프로그램을 컴파일하거나 실행할 때 일반적으로 `swift` 또는 `swiftc`(후자는 전자에 대한 심볼릭 링크)를 호출하는데, 이는 인수에 따라 매우 다른 방식으로 동작할 수 있는 프로그램입니다.
- 직접 코드를 컴파일하거나 실행할 수도 있지만, 보통 **하위 프로세스로 `swift` 또는 `swift` 의 복사본을 하나 이상 실행**합니다. 
- 일반적인 배치 컴파일에서 `swifttc` 의 첫 번째 복사본은 이른바 **driver** 프로세스로, 프로세스 트리에서 **frontend** 로 일컫어지는 다수의 하위 프로세스를 실행하는 역할을 합니다.
- Swift 컴파일을 해석할 때는 어떤 프로세스가 실행되고 어떤 작업을 수행하는지 명확하게 파악하는 것이 중요합니다.

## Driver

- 스위프트 소스 코드를 다양한 컴파일된 결과(executables, 라이브러리, object files, 스위프트 모듈 및 인터페이스 등)로 컴파일하는 프로그램 
- 스위프트 코드를 빌드하기 위해 command line 에서 호출하는 프로그램(i.e., swift or swiftc)이며, 종종 SPM 또는 Xcode의 빌드 시스템 같은 빌드 시스템에 의해 개발자를 대신하여 호출 됨
- **하위 프로세스 트리**에서 **최상위에 위치**한 `swiftc` 프로세스
- 컴파일 및 링킹을 수행하기 위해 **컴파일 또는 재컴파일이 필요한 파일을 결**정하고, **자식 프로세스(소위 jobs )를 실행**하는 책임이 있음 
- 대부분의 실행 동안 하위 프로세스가 완료될 때까지 대기하며 **idle** 상태

## Frontend Jobs

- 드라이버에 의해 시작된 하위 프로세스로,  `swift -frontend ...` 실행 및 컴파일 수행, PCH 파일 생성, 모듈 병합 등을 수행
-  **컴파일 비용의 대부분**을 부담하는 작업들

## Other Jobs

- 드라이버에 의해 시작된 하위 프로세스로,  `ld`, `swift -modulewrap`, `swift-autolink-extract`, `dsymutil`, `dwarfdump` 및 Frontend Jobs 에서 수행한 일괄 작업(batch of work)을 완료하는 데 관련된 유사한 tool 들을 실행
- 이들 중 일부는 `swift` 프로그램도 되겠지만, 이들은 "frontend jobs 을 하는" 것이 아니기 때문에 전혀 다른 프로파일을 갖게 될 것



# 출처

- [CompilerPerformance/Outline of processes and factors affecting compilation performance](https://github.com/apple/swift/blob/main/docs/CompilerPerformance.md#outline-of-processes-and-factors-affecting-compilation-performance)
- [Swift Compiler Driver](https://github.com/apple/swift-driver)
