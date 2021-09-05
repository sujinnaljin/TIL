# App Thinning (feat. App Slicing, On Demand Resource, Bitcode)

- 앱 시닝을 위해서 App Slicing, On Demand Resource, BitCode 의 기술을 사용

## App Slicing

- App Slicing은 **서로 다른 디바이스를 위한** 다양한 앱 번들을 만들고 전달하는 과정
- 앱스토어가 디바이스의 특성을 보고, 필요한것만 조합해서 별도의 IPA를 만듦

![img](https://t1.daumcdn.net/cfile/tistory/998F12455C2F193C08)

## On Demand Resouce (ODR)

- **앱스토어에** IPA와 **별도로 저장**되어 **필요할 때** 마다 다운로드하는 것

![img](https://t1.daumcdn.net/cfile/tistory/99C3A0465C2F1E6A0D)

## Bitcode

- Bitcode를 이용해 다른 아키텍쳐에 대한 최적화를 제거하고 크기를 작게 만들 수 있음

- **LLVM 프론트엔드**에서 작업이 완료되면 **LLVM 비트코드(bitcode)** 가 나오는데 이는 **바이너리로 표현** 되는, 아직 **기계코드도 아니고 내가 이해 할 수 있는 코드도 아닌 중간단계**의 코드

- 이걸 **LLVM 백엔드**에서 **특정 아키텍쳐**(예 : iPhone 6 및 iPad Air 2와 같은 64 비트 프로세서의 경우 arm64)에 맞게 **컴파일**

- 따라서 앱스토어에 **bitcode 를 제출**하면, 다운로드 전 **앱스토어에서 지원하는 백엔드**로 컴파일 가능 (디바이스에 맞게 앱을 최적화 하여 바이너리를 새로 만들어 제공하는 것)

- **bitcode가 나오기 전**까지는, 해당 앱이 실행될 수 있는 모든 환경, 예를들어 32-bit, 64-bit와 arm6/arm7/arm7s/arm64에 대해 바이너리를 생성하여 이를 하나의 파일로 합쳤다고 함 ->  **fat binary**

  

# 출처

- [App Thinning. 그리고 Bitcode](https://zeddios.tistory.com/655)
- [LLVM (feat. 스위프트의 빌드 단계 / 컴파일러 과정)](https://github.com/sujinnaljin/TIL/blob/master/Swift/LLVM.md)

