# iOS 앱에서 사용할 언어를 결정하는 방법

- **앱의 지원 언어 목록**과 **기기의 선호 언어 목록**으로 iOS에서 앱을 표시하는 데 사용할 언어를 결정

- **앱의 지원 언어 목록 중 기기에 설정된 최상위 선호 언어**를 선택하는데, 이때 **엄격한 기준을 적용하진 않음**.  예를 들어 사용자가 가장 **선호**하는 언어가 **스위스 프랑스**어이지만 **앱**이 **캐나다 프랑스**어만 지원하는 경우 **캐나다 프랑스어를 사용**하기로 결정

- 이 조건을 충족하는 언어가 **없으면** `Info.plist`의 **`CFBundleDevelopmentRegion`에 설정된 언어**로 설정

- 만약 `CFBundleDevelopmentRegion` 가 `$(DEVELOPMENT_LANGUAGE)` 로 설정되어 있을때 내 `DEVELOPMENT LANGUAGE`는  `Project > Info > Localizations` 에서 확인 가능

  ![image](https://user-images.githubusercontent.com/20410193/139833871-1eb20e4d-b151-4d69-8740-8e53b5cb4645.png)


# 출처

- [iOS Bad news: Bundle.preferredLocalizations() is not 100% reliable](https://medium.com/@hectorricardomendez/ios-bad-news-bundle-preferredlocalizations-is-not-100-reliable-13fa33454d14)
