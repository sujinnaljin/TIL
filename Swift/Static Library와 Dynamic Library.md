# Static Library와 Dynamic Library

## 정적 라이브러리

- 특정 기능을 달성하기 위해 **컴파일된 소스 코드 파일 모음**
- FileA.swift , FileB.swift , FileC.swift 를 컴파일 하고 함께 래핑하여 **확장자가 [.a](https://stackoverflow.com/a/9810368/11787787) (Archive의 약자)인 정적 라이브러리**를 형성할 수 있음

![image](https://user-images.githubusercontent.com/20410193/141237714-2970af42-ad1c-4a9f-bc37-7b9d5f2b49b3.png)


### 사용하는 이유

- **코드 재사용성**: 여러 앱에 걸쳐 공통된 기능을 가지고 있다고 가정하면, 이러한 **공통 소스 코드를 사용하여 정적 라이브러리를 만들고 모든 앱에서 공유** 가능
- **코드 추상화**: 헤더를 **Private 또는 Internal 로 표시**하여 클라이언트 앱에 알리지 않아야 하는 **코드를 숨길 수** 있음
- **코드 캡슐화**: 정적 라이브러리 바이너리만 배포하면 **기능은 공유하되 구현 세부 정보를 클라이언트에 숨길 수** 있음
- **사용 편의성**: 클라이언트는 기능의 핵심에 대해 걱정할 것이 아니라 최소한의 노력으로 기능을 얻어야 함
- **유지 관리성(Maintainability)**: 모든 코드를 위한 **central place** 가 있으면 버그를 쉽게 수정하고 version bump로 클라이언트와 공유할 수 있음
- **앱 컴파일 시간 단축**: 사전 **컴파일된 정적 라이브러리 바이너리만 배포**하면 해당 기능에 필요한 모든 소스 파일을 컴파일하는 것에 비해 앱의 컴파일 시간을 줄일 수 있음
- **앱 실행 시간 단축**: 동적 프레임워크에 비해 정적 라이브러리가 작동하는 방식은 더 빠른 앱 실행에 도움이 되는 **[dylib 로딩 시간](https://useyourloaf.com/blog/slow-app-startup-times/)과 [Pre-main time](https://useyourloaf.com/blog/slow-app-startup-times/) 을 줄일** 수 있음

### 정적 라이브러리 패키징 흐름

![image](https://user-images.githubusercontent.com/20410193/141237727-2fc18b79-7696-4775-b499-530d139aeb50.png)

### 사용하지 않을 때

정적 라이브러리는 **코드 재사용성과 추상화**에 좋음.  동적 라이브러리 및 동적 프레임워크에 비해서도 단점이 거의 없지만, 아래의 상황을 잘 고려해야함

- **더 큰 앱 크기** : 정적 라이브러리 개체 파일이 **앱 바이너리에 직접 복사**되기 때문에 앱 크기가 부풀려짐. 동적 라이브러리는 앱에 loosely link 되어 있는 반면 정적 라이브러리는 앱에 hard link되어 있음.

  예를 들어 **UIKit과 같은 시스템 프레임워크**는 필요할 때 로드되고 **앱은 참조만 유지**하므로 사용자가 다운로드할 앱 크기에 추가되지 않음

- **리소스** : 정적 라이브러리는 **소스 코드의 모음**일 뿐이므로 많은 기능에 필수적인 **Assets, Xibs, Storyboards 및 JSON과 같은 리소스를 가질 수 없**음

  리소스 번들과 함께 정적 라이브러리를 배포하면 이 문제가 상당 부분 해결되지만 동적 프레임워크는 여기에서 빛을 발함



## Static Library vs Dynamic Library

### 라이브러리 linking

#### Static

- 해당 앱이 사용하는 **라이브러리 코드**가 [Static Linker](https://en.wikipedia.org/wiki/Linker_(computing))에 의해 생성된 **앱 실행 파일(App Executable)에 직접 복사**됨.
- 시스템이 앱을 로드할 때 정적 라이브러리 기능을 **단일 앱 실행 파일로 로드**함. 따라서 로드/실행 시간이 더 빨라짐.

![image](https://user-images.githubusercontent.com/20410193/141237742-d2479471-c489-44b2-b540-7c5b071a5b5a.png)

![image](https://user-images.githubusercontent.com/20410193/141237894-6400ce44-cdb5-40fe-9b77-e7b474d96ebd.png)






#### Dynamic

- Static Linker에 의해 **앱 실행 파일(App Executable)에 라이브러리의 참조(정확히는 이름)만 배치** 됨.

- **실제 linking**은 **앱 실행 파일**(app executable)과 **동적 라이브러리**(dynamic library)가 **메모리에 배치될 때**,  [Dynamic Linker](https://en.wikipedia.org/wiki/Dynamic_linker)에 의해 **launch time** 또는 **run time**에 수행 됨.

  > 동적 라이브러리가 launch time 또는 run time 에 로드될 때, 이 둘의 차이?
  >
  > **launch time** : iPhone에서 해당 앱 아이콘을 탭하면, 시작 화면과 첫 번째 화면을 표시하는 데 필요한 동적 라이브러리와 함께 앱 바이너리가 메모리에 로드됨. 이는 실행 파일(executable)의 *main* 함수 호출과 AppDelegate의 *didFinishLaunchingWithOptions* 에서 *true* 를 반환하는 사이의 시간. 이 시간 동안 앱 상태가 terminated 에서 active 로 전환됨.
  >
  > **run time** : 앱이 실제로 실행되고 사용자가 앱과 상호 작용하는 시간. 이 시간 동안 특정 모듈의 필요에 따라 동적 라이브러리를 로드할 수도 있음. 이는 *didFinishLaunchingWithOptions*의 *return true*와 AppDelegate의 *applicationWillTerminate* 사이의 시간으로, 즉 Launch Time 이후의 시간. 앱 상태는 active에서 terminated 로 전환되기까지.

![image](https://user-images.githubusercontent.com/20410193/141237923-02c9dd2c-301e-46f1-9b50-970e72d1a187.png)

![image](https://user-images.githubusercontent.com/20410193/141237999-c766d607-f47f-443c-af37-90bec5c659a0.png)


### 실행 시간 및 메모리 풋프린트 (Launch Times and Memory Footprints)

#### Static

- **라이브러리 코드가 App Executable에 직접 복사**됨에 따라 많은 정적 라이브러리를 앱에 연결하면 **큰 앱 실행 파일이 생성** 됨.
- **대용량 실행 파일**이 있는 응용 프로그램은 **느린 시작 시간**과 **큰 메모리 공간**으로 어려움을 겪음.

#### Dynamic (aka Dynamically Linked Libraries or DLL)

- 실행 시 또는 런타임에 **실제로 필요할 때 코드가 앱의 주소 공간에 로드** (및 연결)
- 런타임 연결은 더 **빠른 시작 시간**과 더 **적은 메모리 공간**을 제공

### Sharing

#### Static

- 정적 라이브러리 버전 1.0.1에서 1.0.0 의 몇 가지 개선 사항이 추가되었다고 가정

  최신 앱 버전이 1.0이고 정적 라이브러리 버전 1.0.0의 코드가 있다고 가정

  이제 개선된 기능을 사용하려면, **개발자**는 앱의 object file을 **라이브러리의 새 버전 1.0.1로 다시 컴파일(recompile)하고 다시 연결(relink)** 해야 하며, 앱의 **새 버전을 출시** (ex. 1.1) 해야 함

  또한 **앱 사용자**는 최신 버전 1.1로 **앱을 업데이트**해야 함

  따라서 앱을 최신 기능으로 **업데이트**하려면 **앱 개발자와 최종 사용자 모두의 작업이 필요**

#### Dynamic (aka Dynamic Shared Libraries (DSL) & Shared Objects)

- **여러 프로그램**(앱)이 실행 가능한 **동적 라이브러리 모듈의 단일 공유 복사본**을 사용

- 예를 들어, **AppKit (or UIKit) 의 단일 사본은 시스템**에 있고 **모든 앱은 동일하게 가리**킴

- 이렇게 하면 앱 **실행 파일의 크기**가 크게 **줄어**들어 **메모리와 디스크 공간**이 **절약**

- 공유 **라이브러리 코드가 이미 메모리**에 있는 경우 **앱 로드 시간**이 더 **단축**

- 프로그램은 앱 **개발자**가 앱을 **재컴파일할 필요 없**이 자동으로 **사용하는 동적 라이브러리를 개선**하여 이점을 얻을 수 있음. 

  예를 들어  **Mac OS의 모든 시스템 라이브러리가 동적**이기 때문에 Cocoa 기술을 사용하는 앱들은 **Mac OS의 개선으로부터 이점**을 얻을 수 있음.

- 만약 AppKit 동적 라이브러리 버전 1.0.0을 사용하는 앱 버전 1.0이 있다고 가정

  우리의 **앱 실행 파일**은 AppKit에 대한 **참조(이름만)를 유지**

  이제 우리는 OS 업데이트를 받았고 AppKit 버전은 파일 시스템에서 1.0.1로 변경 됨.

  [Dynamic Loader](https://en.wikipedia.org/wiki/Dynamic_linker) 는 **앱 실행 파일을 보고 로드할 AppKit 이름을 찾**는데, **AppKit이 파일 시스템에 존재**하는 한(대부분의 경우 **호환** 가능) **Dynamic Loader는 이를 로드**할 수 있으며 AppKit **버전은 중요하지 않음**

  따라서 **추가 작업을 수행할 필요 없**이 **새 버전** 1.0.1을 받을 수 있음

![image](https://user-images.githubusercontent.com/20410193/141237830-d2d4afb1-68f5-4af3-9a37-0e88fa134c9a.png)

### 실행 시간 속도 (Run time Speed)

- **동적 라이브러리를 사용**하는 프로그램은 특히 앱이 **런타임에 동적 라이브러리 모듈을 연결**하려고 할 때 정적으로 연결된 라이브러리를 사용하는 프로그램보다 **런타임에 느림**
- 이것은 더 **빠른 실행 시간**과 더 **적은 메모리 공간**을 위한 **트레이드 오프**

### 호환성 (Compatibility)

#### Static

- 정적으로 링크된 프로그램에서 **모든 코드**는 **단일 실행 모듈에 포함**. 따라서 **호환성 문제가 발생하지 않**음

#### Dynamic

- 동적 라이브러리를 개발할 때 개발자가 **염두**에 두어야 하는 한 가지 문제는 **라이브러리가 업데이트될 때 클라이언트 앱과의 호환성을 유지하는 것**

- Mac 앱 AdobePhotoshop 버전 1.0 및 AdobeIllustrator 버전 1.0이 모두 공유 동적 라이브러리 AdobeCore 버전 1.0.0을 사용 한다고 가정

  어느날 Adobe 개발자는 AdobeCore 새 버전 1.0.1을 출시하기로 결정. 

  이제 AdobePhotoshop 및 AdobeIllustrator 1.0 은 AdobeCore 1.0.1 을 가리키게 되므로 해당 버전과의 **호환성 검사를 수행**해야 함

  만약 **호환성 문제**가 발생하면 Dynamic Loader가 launch time 또는 run time에 **프로세스(앱)를 종료**

- 클라이언트 앱 개발자 모르게 라이브러리가 업데이트 될 수 있기 때문에, 앱은 **코드 변경 없이 새 버전의 라이브러리를 사용할 수 있어야** 함. 이를 위해 **라이브러리의 API가 변경되어서는 안 됨**

- 다만, 개선을 위해 **API 변경이 필요한 경우**가 있는데, 이 경우 클라이언트 앱이 제대로 실행되려면 **이전 버전의 라이브러리가 사용자의 컴퓨터에 남아 있어야** 함

- 이전 예제에 계속해서 Adobe 개발자는 AdobeCore 1.0.1을 릴리스하고 AdobeCore 1.0.0도 시스템에 유지 하여 다음을 수행할 수 있

  AdobePhotoshop 1.0 및 AdobeIllustrator 1.0은 충돌 없이 AdobeCore 1.0.0을 계속 사용할 수 있으며 최신 AdobePhotoshop (예: 1.1) 및 AdobeIllustrator (예: 1.1)는 개선된 AdobeCore 1.0.1 을 가리킬 수 있음

- **새 컴파일러 릴리즈**는 **라이브러리를 변경할 수 있**으며, 새 버전의 라이브러리와 호환되도록 애플리케이션을 재작업해야 할 수 있음







# 출처

- [Static Library in iOS](https://medium.com/swift-india/static-library-in-ios-d133123678d1)
- [How Jesse Pinkman Cracked Dynamic Library in iOS (Part 1)](https://medium.com/swift2go/how-jesse-pinkman-cracked-dynamic-library-in-ios-part-1-c6be383796b1)
- [Static Libraries vs. Dynamic Libraries](https://medium.com/@StueyGK/static-libraries-vs-dynamic-libraries-af78f0b5f1e4)

