# SKScene

```swift
class RainFall: SKScene {}
```

- 모든 active SpriteKit content 를 구성하는 object 로, SKScene 객체는 **SpriteKit 의 content scene**을 나타낸다. 

- 뷰는 장면들을 전환하여 보여줄 수 있는데, 여기서 SKScene이 **각 장면에 해당하는 클래스**이다.

- 씬은 화면에 등장하는 컨텐츠 구성요소인 **Node들을 관리**하게 되는데, 씬은 SpriteKit 노드([`SKNode`](https://developer.apple.com/documentation/spritekit/sknode))들의 트리에 있는 **루트 노드**이다.

  ![image](https://user-images.githubusercontent.com/20410193/173631335-9b2be8d0-f919-402b-9384-e4b050bb46b0.png)


- 이러한 노드들은 씬(scene)이 애니메이션을 만들고 표시하기 위해 렌더링하는 콘텐츠를 제공한다. 

- scene 을 표시하려면 [`SKView`](https://developer.apple.com/documentation/spritekit/skview), [`SKRenderer`](https://developer.apple.com/documentation/spritekit/skrenderer), 또는 [`WKInterfaceSKScene`](https://developer.apple.com/documentation/watchkit/wkinterfaceskscene) 에서 scene 을 표시한다.

- `SKScene` 은 [`SKEffectNode`](https://developer.apple.com/documentation/spritekit/skeffectnode) 의 하위 클래스로, 특정 효과를 전체 씬에 적용할 수 있다.

- [`SKScene`](https://developer.apple.com/documentation/spritekit/skscene)  을 로드하는 가장 일반적인 방법은 Xcode scene editor 에 정의된 `.sks` 파일을 통해서이ㄷ

**출처**

- https://developer.apple.com/documentation/spritekit/skscene
- http://iconer.iptime.org/~john/wp/index.php/2019/03/23/p-043-skspritenode/
- https://sonej.tistory.com/16
- https://developer.apple.com/documentation/spritekit/skscene/creating_a_scene_from_a_file

## SKScene.sceneDidLoad

- SKScene 에서 `sceneDidLoad` 를 override 해서 scene 이 presented 된 시점을 캐치할 수 있다

  ```swift
  class RainFall: SKScene {
      override func sceneDidLoad() {
      }
  }
  ```

**출처**

- https://developer.apple.com/documentation/spritekit/skscene/1645216-scenedidload

## SKScene.scaleMode

- 씬(scene)의 크기는 씬(scene)이 표시되는 뷰의 크기와 다를 수 있기 때문에 `scaleMode` 로 씬(scene)의 보이는 부분이 뷰에 매핑되는 방법을 결정할 수 있다. 

- 기본값은 [`SKsceneScaleMode.fill`](https://developer.apple.com/documentation/spritekit/skscenescalemode/fill) 이고 `aspectFill`, `aspectFit` , `resizeFill` 등이 있다

- .aspectFit 을 선택하면 아래와 같이 보임

  > aspectFit - 각 디멘션의 scaling factor가 계산되고 둘 중 더 작은 것이 선택됨. 씬(scene)의 각 축은 동일한 scaling factor에 의해 scaling 됨. 이렇게 하면 전체 씬(scene)을 볼 수 있지만 뷰에서 letterboxing가 필요할 수 있음.) 

```swift
class RainFall: SKScene {
    override func sceneDidLoad() {
        size = CGSize(width: UIScreen.main.bounds.size.width - 100,
                      height: UIScreen.main.bounds.size.height)
        scaleMode = .aspectFit
    }
}
```

![image](https://user-images.githubusercontent.com/20410193/173631493-b4c4b47b-6649-4c74-9c99-355378c6d2dd.png)

```swift
class RainFall: SKScene {
    override func sceneDidLoad() {
        size = CGSize(width: UIScreen.main.bounds.size.width - 100,
                      height: UIScreen.main.bounds.size.height - 400)
        scaleMode = .aspectFit
    }
}
```

![image](https://user-images.githubusercontent.com/20410193/173631538-f25d831c-57f3-4b4b-8564-cc39cc65f85d.png)

- resizeFill 을 선택하면 아래와 같이 보임

  > resizeFill - 씬(scene)의 크기가 뷰에 맞게 조정되지 않음. 대신 씬(scene)의 dimension이 항상 뷰의 dimension과 일치하도록 씬(scene)의 크기가 자동으로 조정됨

```swift
        size = CGSize(width: UIScreen.main.bounds.size.width - 100,
                      height: UIScreen.main.bounds.size.height - 400)
        scaleMode = .resizeFill
```
![image](https://user-images.githubusercontent.com/20410193/173631610-c5b5581a-d3c8-4878-80e2-2853732fdb6e.png)


**출처** 

- https://developer.apple.com/documentation/spritekit/skscene/1519562-scalemode

## SKScene.anchorPoint

```swift
class RainFall: SKScene {
    override func sceneDidLoad() {
        size = UIScreen.main.bounds.size
        anchorPoint = CGPoint(x: 0.5, y: 1)
    }
}
```

- 씬(scene)의 origin에 해당하는 뷰 프레임의 point

- 씬(scene)이 표시되고 카메라 노드 ([`var camera: SKCameraNode?`](https://developer.apple.com/documentation/spritekit/skscene/1519696-camera))가 지정되지 않은 경우에는  [`size`](https://developer.apple.com/documentation/spritekit/skscene/1519831-size) 와 [`anchorPoint`](https://developer.apple.com/documentation/spritekit/skscene/1519864-anchorpoint) 프로퍼티에 따라 씬(scene) 좌표 공간 (coordinate space) 의 어느 부분이 뷰에 표시되는지 결정됨

- 씬(scene)이 항상 노드 트리의 루트 노드이기 때문에 씬(scene)의[`position`](https://developer.apple.com/documentation/spritekit/sknode/1483101-position) 는 Scene Kit 에서 무시됨.  기본값은[`zero`](https://developer.apple.com/documentation/coregraphics/cgpoint/1454433-zero)이며 변경할 수 없음

  그러나[`anchorPoint`](https://developer.apple.com/documentation/spritekit/skscene/1519864-anchorpoint) 속성을 설정하여 씬(scene)의 origin을 이동할 수 있음. 

- unit coordinate space을 사용하여 값을 지정. 씬의 visible coordinate space 은  `(0,0)` 부터 `(width,height)` 까지

- 기본값은 뷰의 프레임 직사각형의 왼쪽 하단 모서리에 해당하는 (0,0)

- 만약 0.5, 0.5 로 세팅하면 node 를 screen 의 center 로 놓을 수 있음

  ![스크린샷 2022-06-15 오전 1 46 10](https://user-images.githubusercontent.com/20410193/173631963-e1b0991a-a61e-49b0-b56e-e9f9ea57aded.png)


```swift
class RainFall: SKScene {
    override func sceneDidLoad() {

        
        size = UIScreen.main.bounds.size
        scaleMode = .resizeFill
        
        // anchor point..
        anchorPoint = CGPoint(x: 0.5, y: 0.5)
        
        // bg color...
        backgroundColor = .clear
        
        // creating node and adding to scene..
        let node = SKEmitterNode(fileNamed: "RainFall.sks")!
        addChild(node)
    }
}
```

![image](https://user-images.githubusercontent.com/20410193/173631676-0bc76dd4-88e8-4d5c-97a9-e5a79f0e30d4.png)

**출처**

- https://developer.apple.com/documentation/spritekit/skscene/1519864-anchorpoint
- https://developer.apple.com/documentation/spritekit/skscene/positioning_a_scene_s_origin_within_its_view

## SKScene.backgroundColor

- scene 의 백그라운드 컬러
- 기본 컬러는 회색인 RGBA `0.15, 0.15, 0.15, 1.0`

![image](https://user-images.githubusercontent.com/20410193/173631709-4aff0584-e16d-4c76-9669-e5dd0ba74731.png)

**출처**

- https://developer.apple.com/documentation/spritekit/skscene/1520278-backgroundcolor

## SKScene.addChild

- receiver 의 하위 노드 목록 마지막에 노드를 추가

- 파라미터로 `SKNode` 타입의 인스턴스 추가 가능

  ```
  func addChild(_ node: SKNode)
  ```

- 추가할 노드는 parent 에 이미 존재하면 안됨

**출처**

- https://developer.apple.com/documentation/spritekit/sknode/1483054-addchild
