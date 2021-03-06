# ARM64 레지스터

```assembly
$x0 = ORRXrs $xzr, $x20
BL swift_release
```

`$xzr` (zero register)로 비트 OR 연산 (ORR 명령어) 을 수행하여 레지스터 `$x20` (source register) 에 있는 값을 레지스터 `$x0` (destination register) 로 이동한다.

`BL swift_release` 는 첫번째 인자로 `$x0` 을 받는 듯 하다. 그래서 `$x20` 에 있던 포인터 (heap object 를 가리키던) 를 해제시키려면 우선 `$x0` 으로 옮겨야 하는 듯.

참고로 Swift 5부터 64 비트 아키텍처에 대한 호출 규칙 레지스터 할당 은 [ABI 안정성 선언문](https://github.com/apple/swift/blob/master/docs/ABIStabilityManifesto.md) 에 설명 된 Context register 및  Error return register 를 추가하여 [기존 ](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/iPhoneOSABIReference/Articles/ARM64FunctionCallingConventions.html)[표준](https://developer.apple.com/library/content/documentation/DeveloperTools/Conceptual/LowLevelABI/140-x86-64_Function_Calling_Conventions/x86_64.html) 을 기반으로 한다.

| Register Purpose        | ARM64   | x86_64                     |
| ----------------------- | ------- | -------------------------- |
| Context register (self) | x20     | r13                        |
| Error return register   | x21     | r12                        |
| Struct return pointer   | x8      | rax                        |
| Float call arguments    | v0 … v7 | xmm0 … xmm7                |
| Integer call arguments  | x0 … x7 | rdi, rsi, rdx, rcx, r8, r9 |
| Float return            | v0 … v3 | xmm0 … xmm3                |
| Integer return          | x0 … x3 | rax, rdx, rcx, r8          |
# 출처

- [How Uber Deals with Large iOS App Size](https://eng.uber.com/how-uber-deals-with-large-ios-app-size/)

- [64-Bit Architecture Register Usage](https://github.com/eaplatanios/swift-language/blob/master/docs/ABI/RegisterUsage.md)

