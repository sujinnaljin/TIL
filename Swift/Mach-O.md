# Mach-O

- Mach-O는 Mach Object file format의 약자 (Mach는 커널)

- 실행 파일, 오브젝트 코드, 공유 라이브러리, 동적으로 로드 된 코드 및 코어 덤프 등의 **파일 형식**

  즉, kernel extensions, command-line tools, applications, frameworks, 그리고 libraries (shared and static)는 Mach-O 파일을 사용하여 구현

- iOS와 macOS 계열에서 사용

  ![img](https://k.kakaocdn.net/dn/QuuP9/btqAgT0lJBP/T6U8jNENUGWV5HEhaoTCK1/img.png)

## Mach-O Type

- 바이너리의 타입을 지정하는 identifier

- Single View App이나 command line tool은 기본적으로 **Executable**

- 내가 만들고 싶은 응용프로그램에 맞게 이 Mach-O Type을 지정해야 함

  Ex. 

  - Framework -> Dynamic Library

  - Static Library -> Static Library

  

  ![img](https://k.kakaocdn.net/dn/c0G1fU/btqAhVciLMp/skBa8H9Ble3RJP7lUIyVv0/img.png)

  

# 출처

- [Mach-O](https://zeddios.tistory.com/908)
- [Mach-O](https://en.wikipedia.org/wiki/Mach-O)
- [iOS 코드 서명 (feat. 프로비저닝 프로파일과 Mach-O 파일)](https://github.com/sujinnaljin/TIL/blob/master/Swift/iOSCodeSigning.md)

