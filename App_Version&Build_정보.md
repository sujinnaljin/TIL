# App Version & Build 정보

![](https://i.stack.imgur.com/GK48e.png)

- iOS에서는 현재 사용자가 실행 중인 앱의 버전을 확인하고 업데이트를 유도할 수 있는데 이에 해당하는 정보는 **프로젝트의 General에서 App의 Version과 Build 번호**로 확인할 수 있다
- 빌드해서 앱스토어에 올라가는 바이너리에는 해당 번들 정보를 갖고 있다

- Version은 배포할 때마다 바뀌고 보통 x.x.x 의 형태를 사용한다. 바뀌는 번호가 앞에 있을수록 변경 사항이 크다.

- Build 번호는 같은 Version에서 변경 사항이 있을때 올린다. 예를 들어 핫픽스..? 근데 회바회 사바사인듯?
- 아래와 같이 version과 build를 코드로 확인할 수 있다.

```swift 
let version = Bundle.main.object(forInfoDictionaryKey: "CFBundleShortVersionString") as! String
let build = Bundle.main.object(forInfoDictionaryKey: kCFBundleVersionKey as String) as! String
```

