# dSYM (Debug SYMbol)

## TL;DR;

- DSYM 파일은 **빌드 ( 컴파일 + 실행파일을 만드는 과정 ) 시 고유한 DSYM 파일**이 생성됨
- 앱스토어에서 크러쉬 로그를 보내줄시 **DSYM 파일이 존재하지않는다면 난독화되어있는 파일을 해석할수 없**음
- 또한 난독화를 해제(애플에선 de-obfuscate 표현)할수 있다면, **소스 코드상에서 크러쉬가 난 소스 라인 및 함수 이름까지 파악** 가능

## dSYM 이란

- 앱이 충돌하면 운영 체제는 충돌 당시 앱이 무엇을 하고 있었는지에 대한 진단 정보를 수집함. 충돌 보고서의 가장 중요한 부분 중 하나는 16진수 주소로 보고되는 **스레드 역추적**(Thread backtraces) 임.

  unsymbolicated crash report의 예시

  ![img](https://miro.medium.com/max/1400/1*FCErE8uhSFpG2YHg4Ik2hA.png)

- 이제 앱 충돌의 근본 원인을 이해하려면 먼저 **스레드 역추적**을 **사람이 읽을 수 있는** 언어 (즉 , 충돌을 일으킨 소스 코드의 **함수 이름**과 **줄 번호**)로 변환해야하는데, 이 과정을 **SYMBOLICATION** 라고 함.

- **dSYM 파일**은 앱의 **debug symbol을 저장**함. 여기에는 **stack-trace를 readable format으로 디코딩**하기 위한 매핑 정보가 포함 되어 있음.

- dSYM의 목적은 **충돌 로그의 symbol을 특정 메서드 이름으로 교체**하여 읽기 쉽고 충돌 디버깅에 도움이 되도록 하는 것

- dSYM이 없으면 충돌 보고서에는 개체 및 메서드의 메모리 주소만 표시 됨. 바이너리 파일을 먼저 다시 re-symbolicating하지 않으면 충돌 보고서를 읽을 수 없음 -> 따라서 앱 **크래쉬 리포트를 해석**하기 위해선 반드시 **dSYM이 포함된 아카이브를 잘 관리**해야함

- dSYM 파일은 Build Settings -> 'DEBUG_INFORMATION_FORMAT'이 **`DWARF with dSYM file`** 로 설정되어 컴파일 될때 생성 (컴파일 시간을 줄이기 위해 `DWARF with dSYM File` 대신 `DWARF`로  설정하는 것이 좋음)

- dSYM 파일은 코드가 변경된 앱을 컴파일할 때마다 변경 됨.

- 참고로 **프레임워크**의 경우 **Mach-O**에 따라 **dSYM 생성 여부**가 결정됨. **Static Framework**의 경우, 최종 결과물인 앱의 바이너리 안에 포함되기에 **dSYM 파일이 생성되지 않고**, **Dynamic Framework**의 경우, **dSYM 파일이 생성** 됨.

- dSYM 파일은 Xcode의 메뉴에서 `Window - Organizer` 에서 아카이빙 된 파일들을 찾아 `Show in Finder`로 xcarchive파일을 확인할 수 있음. 여기서 패키지 내용보기를 통해 dSYM 파일을 얻을 수 있음. 또는 `Xcode - Preferences - Locations - Archives` 에서 경로를 확인할 수 있음.



# 출처

- [How Crashlytics works in iOS? An Overview of dSYM](https://medium.com/naukri-engineering/overview-of-dsym-crashlytics-in-ios-dfd72eae8b58)
- [iOS dSYM 이란 무엇인가](https://jjhyuk15.medium.com/ios-dsym-%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-69516fa7ce99)
- [[Xcode]dSYM 파일은 어디 있나요?](http://minsone.github.io/mac/ios/where-is-the-dsym-file-in-xcode)

