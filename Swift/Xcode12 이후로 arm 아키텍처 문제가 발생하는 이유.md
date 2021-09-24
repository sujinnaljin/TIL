# Xcode12 이후로 arm 아키텍처 문제가 발생하는 이유

## Xcode에서 빌드와 관련한 배경 지식

1. 아이폰은 **기종에 따라 각기 다른 아키텍쳐의 CPU**를 사용

   ```
   - arm64 : 아이폰 5S 및 이후 기종 (6, 6S, SE, 7…) 및 arm cpu 맥 시뮬레이터
   - arm7s : 아이폰 5, 5C
   - arm7 : iOS7 지원 옛날 기기
   - x86_64 : 인텔 cpu 맥 시뮬레이터
   ```

2. 빌드시 바이너리가 생성되는데, 이 바이너리는 각 CPU 아키텍쳐에 맞게 생성 필요 (아키텍쳐별로 기계어 명령어 셋 등이 다름)

## Xcode12 이전

- 하나의 앱을 **아이폰 4S에서부터 최신기기까지 동작**하도록 하려면 **`arm64`, `arm7`, `arm7s` 바이너리를 각각 생성**해 원하는 모든 기기에서 동작할 수 있도록 해야 함.
- Xcode에서 이와 관련해 설정하는 부분은 `Build Setting -> Valid Architectures` 에 있었음

![img](https://jusung.github.io/images/2020/Xcode%20Build%20Error2.png)*Valid Architectures 설정(Xcode11)*

- 이렇게 설정하면 빌드시 **Valid Architectures로 지정한 모든 아키텍쳐 바이너리를 생**성해 앱이 각 아키텍쳐를 사용하는 모든 기기에서 동작
- 따라서 Xcode에 아이폰11 기기를 연결하고 debug 설정으로 Run을 실행하면 앞에서 살펴본대로 Valid Architecture에 지정한 **`arm64`, `arm7`, `arm7s` 용 빌드를 생성**해 기기에 심음.

![img](https://jusung.github.io/images/2020/Xcode%20Build%20Error4-0.png)

- 그런데 '**아이폰11은 `arm64` 아키텍쳐를 사용**하는데, 아이폰 11에다가 빌드하면 `arm64` 바이너리만 만들면 되지 **굳이 `arm7`, `arm7s` 바이너리까지** 만들 필요가 있을까?' 라는 생각이 들 수 있음 -> **이럴때 사용하기 위한 옵션**이 **`Build Active Architecture Only`**

![img](https://jusung.github.io/images/2020/Xcode%20Build%20Error5.png)

- Build Setting의 이 옵션을 활성화 시키면 Xcode에서 빌드시 Valid Architectures를 참조하지 않고, **현재 연결된 기기를 감지해 그 기기에 맞는 아키텍쳐용 빌드만 생성**

- **아이폰 시뮬레이터** 같은 경우는, 맥에서 동작하기 때문에 **맥 CPU의 아키텍쳐**를 따름. 따라서 **인텔 CPU**를 사용하는 맥의 시뮬레이터는 **`x86_64` 아키텍쳐**를 사용.

![img](https://jusung.github.io/images/2020/Xcode%20Build%20Error4.png)

- 여기까지가 **Xcode12 이전**에 빌드 수행시 기기 및 시뮬레이터에서 동작하던 방식

## Xcode12 이후

 - Xcode12 에서 동작하는 방식크게 세 가지 변경 사항 존재
   1. Build Setting에 `Valid Architectures(VALID_ARCHS)`가 더 이상 포함되지 않음 (이 설정을 사용하는 것을 장려하지 않음)
   2. Valid Architectures 설정 대신 **`EXCLUDED_ARCHS` 설정**이 생김
   3. 기존 Valid Architectures 내용은 사용자 정의 섹션(User-Defined)에 노출됨

![img](https://jusung.github.io/images/2020/Xcode%20Build%20Error7.png)*`Valid Architectures(VALID_ARCHS)` 설정이 더 이상 `Architectures`섹션에 있지않고 `User-Defined`섹션에 노출된 것을 확인하실 수 있습니다*

- 기존에는 빌드시 **원하는 아키텍쳐를 지정**하는 방식을 사용했는데, Xcode12에서는 반대로 빌드시 **제외시킬 아키텍쳐를 지정**하는 방식을 사용(필수x, 권장하는 부분)
- 그 이유는 `arm64`구조를 사용하는 **ARM 기반 맥북** 때문.
- Xcode12는 **두개 아키텍쳐를 모두 지원**.
- 전에는 시뮬레이터를 선택 후 빌드를 하면 **`x86_64`용 빌드만** 했는데, Xcode 12에서 시뮬레이터는 `x86_64`, `arm64`를 다 지원하기때문에 **`x86_64`용과, `arm64`용으로 빌드**를 수행. 
- 이 과정에서 **현재 맥은 `x86_64`용 맥이기 때문에 `arm64`용 빌드 수행시 에러**가 발생.
- 빌드시 이 에러를 막기 위해서는 **시뮬레이터 빌드시 `arm64`용 빌드를 진행하지 않도록** 해야함 
- 이 설정을 Xcode12 에서 새롭게 추가된 **`EXCLUDED_ARCHS`에 설정** 가능. 
- Build Setting에 `EXCLUDED_ARCHS` 항목에 "빌드를 시뮬레이터에 하는 경우에는 arm64 바이너리용 빌드는 하지 말아줘."라고 지정을 하면 빌드시 `arm64` 빌드를 수행하지 않아 에러가 발생하지 않음

![img](https://jusung.github.io/images/2020/Xcode%20Build%20Error9.png)*EXCLUDED_ARCHS 변수를 설정하면 Xcode에서 Excluded Architectures 섹션에 노출*

- 참고로 엑코 12 부터 **Carthage에서 빌드한 Framework도 arm64로 빌드**가 되는 현상이 발생하기 때문에 `EXCLUDED_ARCHS` 에 `arm` 아키텍처를 set 해주는 build script 작성해야함 ([Carthage 빌드 in Xcode 12](https://github.com/sujinnaljin/TIL/blob/master/Swift/Carthage%20%EB%B9%8C%EB%93%9C%20in%20Xcode%2012.md))

## 출처

- [[Xcode] Xcode12에서 시뮬레이터 빌드 오류 원인 및 해결방법](https://jusung.github.io/Xcode12-Build-Error/)

