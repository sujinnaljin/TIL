# Compile vs Build vs Run

## Compile 

- 컴파일러는 특정 프로그래밍 언어로 쓰여 있는 문서를 다른 프로그래밍 언어로 옮기는 언어 번역 프로그램

- 즉, **사람이 작성한 언어를 기계가 읽을 수 있도록 번역**하는 것

- Xcode 에서는 Swift Compiler 설정이랑  Apple Clang 설정이 각각 있는 것을 볼 수 있음

  **for Swift**

  <img width="932" alt="image" src="https://user-images.githubusercontent.com/20410193/163541076-54b62fa1-29f4-4a39-a046-f9bc5a62fa18.png">
  
  **for C family**

  <img width="932" alt="image" src="https://user-images.githubusercontent.com/20410193/163541052-1ed477d5-3b30-4576-9103-10379cd12929.png">



## Build 

- 소스 코드 파일을 컴퓨터나 휴대폰에서 실행할 수 있는 독립 소프트웨어 가공물로 변환하는 과정을 말하거나 그에 대한 결과물
- Xcode 명령어 - Command + B
- 즉, **문서나 리소스 등을 하나로 합쳐 앱으로 실행되게끔 하는 과정이나 앱**
- 하나의 앱으로 실행되려면 기계가 읽을 수 있게끔 번역하는 과정이 필요기때문에, **컴파일은 빌드 안에 포함되어 있는 과정**


## Run

- 어떠한 제품을 실제 장치에서 실행시키는 프로세스
- Xcode 명령어 - Command + R
- **즉, 빌드가 완성된 앱을 실제 시뮬레이터나 디바이스에 실행시키는 과정**
- 빌드 안에 컴파일이 포함되어 있고 런안에 빌드가 포함되어 있는 것


<img width="982" alt="image" src="https://user-images.githubusercontent.com/20410193/163541100-41724958-1b1b-4236-9756-34cbda35c02e.png">



# 출처

- [[OS] 컴파일과 링크 그리고 빌드와 실행의 차이는?(Differences Build,Complile,Run and Link)](https://fomaios.tistory.com/entry/CS-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EA%B3%BC-%EB%A7%81%ED%81%AC-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EB%B9%8C%EB%93%9C%EC%99%80-%EC%8B%A4%ED%96%89%EC%9D%98-%EC%B0%A8%EC%9D%B4%EB%8A%94Differences-BuildComplileRun-and-Link)
- [[iOS\] XCode Build System 이해하기](https://eunjin3786.tistory.com/323)
- [Xcode Build Configuration 설정하기](https://zeddios.tistory.com/705)

