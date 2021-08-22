# ScenePhase

- **scene의 operational 상태** 

  ```swift
  @Environment(\.scenePhase) private var scenePhase
  ```

## Scene Phase 종류

- Scene Phases 는 아래와 같은 **세가지 종류**가 있음.

### `active`

- **foreground & interactive**
- active scene 이라고 해서 반드시 **최상위 (front-most) 에 있을 필요는 없음**. (ex. macOS 윈도우에 현재 포커스가 없는 상태일지라도 active한 상태일 수 있음)
- **`App`** 또는 **`Custom Scene`**에서 active phase라는 것은 **active scene 인스턴스가 하나 이상 포함**되어 있는 것을 의미

### `inactive`

- **foreground & should pause** its work
- **이벤트를 수신하지 않으며** 타이머를 **일시 중지**하고 **불필요한 리소스를 해제**(free) 해야 함
- scene이 사용자 인터페이스에서 **완전히 숨겨**지거나, 사용자가 **사용하지 못하는 상황**일 수 있음
- macOS에서 scene은 `background` 단계로 이동하는 동안에만 일시적으로 이 단계를 거침
- **`App`** 또는 **`Custom Scene`**에서 inactive phase라는 것은 **active scene 인스턴스가 하나도 없다**는 것을 의미

### `background`

- **현재 visible 하지 않은 상태**
- `background` 단계에 있는 scene에서 무언가를 하는 것은 가능한 적어야 함
- `background` 단계로 진입하면 **앱이 종료될 것으로 예상** 할 수 있음. 즉, **종료 전에 발생 가능**하므로 이 상태로 전환되는 즉시 **정리 작업을 수행** (ex. 열려 있는 파일 및 네트워크 연결을 모두 닫기)
- 하지만 `background`에서 `active` 단계로 돌아갈 수도 있음

### Test

- 앱이 **켜진 직후** -> `active`
- 앱 **스위칭 할 수 있게 올린** 상태 -> `inactive`
- 아이폰 **홈화면**으로 나왔을 때 -> `background`
- 앱 **재진입**시 -> `inactive` -> `active`

## scenePhase 값을 해석하는 방법

- scenePhase 값을 **해석하는 방법**은 **값이 어디서 읽히느냐에 따라 다름** (`View` vs `App` vs `Custom Scene`)

### View 

- View instance 내부에서 phase를 읽으면 **뷰가 포함된 씬(scene)의 phase**를 얻을 수 있음

```swift
struct MyView: View {
    @ObservedObject var model: DataModel
    @Environment(\\.scenePhase) private var scenePhase

    var body: some View {
        TimerView()
            .onChange(of: scenePhase) { phase in
                model.isTimerRunning = (phase == .active)
            }
    }
}
```

### App

- `App` instance 내부에서 phase를 읽으면 **앱의 모든 씬(scene)의 phase을 반영하는 집계(aggregate) 값**을 얻을 수 있음
- active scene이 있으면 앱에서 `active`의 값을, active scene이 없으면 `inactive`의 값을 보냄
- **single scene(ex. WindowGroup)에서 생성된 여러 scene 인스턴스가 포함**

```swift
@main
struct MyApp: App {
    @Environment(\\.scenePhase) private var scenePhase

    var body: some Scene {
        WindowGroup {
            MyRootView()
        }
        .onChange(of: scenePhase) { phase in
            if phase == .background {
                // Perform cleanup when all scenes within
                // MyApp go to the background.
            }
        }
    }
}
```

### Custom Scene

`custom scene` 인스턴스 내에서 phase를 읽으면 **custom scene을 구성하는 모든 scene의 phase 를 반영하는 집계 값**을 유사하게 얻을 수 있음

```swift
struct MyScene: Scene {
    @Environment(\\.scenePhase) private var scenePhase

    var body: some Scene {
        WindowGroup {
            MyRootView()
        }
        .onChange(of: scenePhase) { phase in
            if phase == .background {
                // Perform cleanup when all scenes within
                // MyScene go to the background.
            }
        }
    }
}
```



# 출처

- [ScenePhase](https://developer.apple.com/documentation/swiftui/scenephase)
- [ScenePhase.active](https://developer.apple.com/documentation/swiftui/scenephase/active)
- [ScenePhase.inactive](https://developer.apple.com/documentation/swiftui/scenephase/inactive)
- [ScenePhase.background](https://developer.apple.com/documentation/swiftui/scenephase/background)
- [[iOS14][SwfitUI] SwiftUI2 App life cycle 정리](https://huniroom.tistory.com/entry/iOS14SwfitUI-SwiftUI-life-cycle-%EC%97%90%EC%84%9C-%EB%94%A5%EB%A7%81%ED%81%AC-%EC%B2%98%EB%A6%AC)

