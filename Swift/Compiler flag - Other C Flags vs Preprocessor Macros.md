# Compiler flag - Other C Flags vs Preprocessor Macros

- 둘 다 컴파일러 플래그를 세팅할 수 있는 방법임 (objetive c 파일 용도. swift 는 other swift flags 에서 설정)

  - **Other C Flags** ( `OTHER_CFLAGS` )
    
    `FOO` 를 전달하려면 `-DFOO` 로 설정해야함. 여기서 `-D` 는 `#define` 을 세팅하는 gcc flag 임. `-DTWO=2` 는 `#define TWO 2` 가 되는 것
    
    <img width="475" alt="스크린샷 2023-04-19 오전 12 01 27" src="https://user-images.githubusercontent.com/20410193/232818721-040ec1e7-be9c-4a84-a377-ce7acdf531d7.png">

  - **Preprocessor Macros** ( `GCC_PREPROCESSOR_DEFINITIONS` )
    
    `FOO` 를 전달하려면 `FOO` 로 설정해야함 (`-D` 가 자동으로 붙음)
    
    <img width="454" alt="스크린샷 2023-04-19 오전 12 00 25" src="https://user-images.githubusercontent.com/20410193/232818452-b51232f9-d7e8-4bc6-9fee-e8cf7440e13a.png">


-  `FOO` 와 같은 상수로 정의하는 것은 (constant definition) 는 기본적으로 bool 값을 나타냄. 
    
    <img width="491" alt="image" src="https://user-images.githubusercontent.com/20410193/232818355-326edb63-ddd1-49f7-9bf6-3fccbd7d2b7a.png">


-  `FOO=1` 과 같은 값 정의(value defitnition) 를 할 수도 있는데, 보통 복잡한 케이스를 원하지 않기 때문에 `FOO=1` 정도로 플래그의 존재를 확인하는 정도로만 사용
   
   <img width="454" alt="스크린샷 2023-04-19 오전 12 00 25" src="https://user-images.githubusercontent.com/20410193/232818452-b51232f9-d7e8-4bc6-9fee-e8cf7440e13a.png">


- 그럼 실제 코드에서는 아래와 같이 플래그 사용 가능

  ```
  #if NALJIN
  static NSString *const MY_API_URI = @"https://api.example.com/";
  #else
  static NSString *const MY_API_URI = @"https://api.staging-example.com/";
  #endif
  ```

# 출처

- [Xcode Build Settings Part 1: Preprocessing](https://thoughtbot.com/blog/xcode-build-settings-part-1-preprocessing)

- [why we keep -D in other c Flags in xcode](https://stackoverflow.com/questions/4508898/why-we-keep-d-in-other-c-flags-in-xcode)
