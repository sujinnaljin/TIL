# Metal

- Apple이 개발한 **그래픽 API**

- Metal API는 Metal 프레임워크, MetalKit 프레임워크, Metal Performance Shaders 프레임워크, Metal shading language, Metal 정규 라이브러리를 모두 포함

- **OpenGL**의 오버헤드로부터 벗어나 **더 나은 성능**을 제공

  > OpenGL (Graphic Library) - 컴퓨터 **그래픽을 하드웨어 가속으로 처리**(렌더링)함과 동시에 여러 분야에서 사용될 수 있도록 하는 범용성을 보장하기 위해 발표된 **2D, 3D 그래픽 라이브러리**이자 산업용 API
  >
  > 예를 들어, 3D 를 만들때 입체감을 살리기 위해서는 물이면 물 텍스쳐를 살린다던가, 빛을 준다던가, 그림자의 변화를 준다던가.. 등의 작업이 필요한데 이는 굉장히 복잡하고 많은 연산이 필요함. 변환, 채색, 조명, 텍스처, 그림자 등 변화를 주는 단계가 많고 수학적인 계산도 요구되어, 소프트웨어만으로는 한계가 있으며 **하드웨어 가속 기능**이 필수적인데, 이런 **복잡한 연산과 하드웨어 제어를 대신해 주는 것이 바로 3D 그래픽 라이브러리**. 그 중 하나가 **OpenGL**

- Metal 의 장점은 아래와 같음

  - **OpenGL의 그래픽**과 **OpenCL의 계산**을 **통합 API**로 결합함.
  - 앱에서 **멀티스레드 렌더링** 사용가능
  - **낮은 오버헤드**
  - **고효율/고성능** GPU 프로그래밍 API
  - Metal의 shading language는 C++기반이며, 앱에서 사용된 모든 shaders를 precompile할 수 있으므로, 다양한 재질의(material) shaders를 쉽게 만들 수 있음.

- Apple의 고수준 그래픽 라이브러리인 **Core Graphics, Core Animation 역시 기존에 OpenGL을 베이스**로 하고 있었으나, **OS X El Capitan 이후 모두 Metal에서 작동**

- 앱이 SpriteKit, SceneKit, RealityKit, Core Image, Core Animation과 같은 레이어 위에 구축 된 경우 이미 Metal을 사용하고있는 것이며, 애초에 **UIKit이 Metal 위**에 있음

  ![img](https://blog.kakaocdn.net/dn/0bdKl/btqARYtIypb/cNUruhoPV6lgPLyZ4CzIDK/img.png)

- WWDC 2014에서 iOS 8과 함께 발표되었으며 A7 및 이후 출시되는 **AP를 탑재한 모든 iOS 모델**이 지원 대상에 해당. WWDC 2015에서 macOS용으로도 발표되어 **2012년 이후 발매된 모든 Mac**에서 지원. 

- Apple은 2018년 출시된 macOS 10.14 **모하비부터 OpenGL 및 OpenCL의 지원을 중단**하겠다고 공언함으로서 공식적으로 **Apple의 운영체제의 그래픽 라이브러리는 Metal을 제외하고 모두 공식적으로 퇴출** 됨

- 언리얼 엔진과 유니티 엔진 역시 Metal API를 지원하며, 프리미어 프로도 Metal을 지원하여 Windows 노트북보다 대등하거나 더 좋은 렌더링 성능을 기대할 수 있게 됨.



# 출처

- [Metal(API)](https://namu.wiki/w/Metal(API))

- [OpenGL](https://namu.wiki/w/OpenGL)

- [Metal이 뭔지 궁금해서 쓰는 글](https://zeddios.tistory.com/932)

- [Metal](https://developer.apple.com/kr/metal/)

  

