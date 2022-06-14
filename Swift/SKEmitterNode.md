# SKEmitterNode

- 다양한 입자(particle) 효과의 source
- `SKEmitterNode` object 는 **작은 입자 스프라이트를 자동으로 만들고 렌더링**하는 노드
- 파티클 효과를 사용하여 반짝이거나 연기가 나는 사실적인 불과 같은 특수 효과를 앱에 추가. Emitter 노드는 **연기, 불, 불꽃 및 기타 입자 효과**를 만드는 데 자주 사용됨.  
- 파티클의 동작은 파티클을 생성한 emitter 노드에 의해 정의 됨. SKEmitterNode에는 다음과 같은 파티클의 동작을 제어하는 많은 속성이 포함되어 있음. emitter 노드를 구성하는 데 사용되는 속성의 전체 목록은 [`SKEmitterNode`](https://developer.apple.com/documentation/spritekit/skemitternode)에 나와 있음
  - 입자(particle)의 birth rate 와 lifetime. 입자가 렌더링되는 순서와 emitter가 꺼지기 전에 생성되는 최대 입자 수를 지정할 수도 있음.
  - 위치, 방향, 색상 및 크기를 포함한 입자의 시작 값. 이러한 시작 값을 무작위로 지정하도록 선택할 수 있음
  - lifetime 동안 입자에 적용할 변경 사항. 일반적으로 시간 경과에 따른 변화율(rate of change)로 지정됨. 예를 들어 입자가 초당 라디안 단위의 특정 속도로 회전하도록 지정할 수 있음. 

- 이를 코드로도 지정할 수 있지만 Xcode의 SpriteKit Particle Editor를 사용해서 particle 효과를 만들고 동일한 속성 값을 설정할 수 있음
- Editor를 사용하면 코드를 복제하지 않고도 앱의 다른 위치에서 particle를 반복할 수 있다는 이점이 있음

- 입자가 생성될 때 초기 속성 값은 emitter의 속성에 의해 결정됨. 각 입자의 속성에 대해 emitter 클래스는 최대 4개의 속성을 선언.
  - 속성의 평균 시작 값(Start)
  - 속성 값의 임의 범위(Range). 새 입자가 방출될 때마다 해당 범위 내에서 새 임의 값이 계산됨.
  - 속성의 속도(Speed). 즉, 시간 경과에 따라 값이 변경되는 비율. 모든 속성에 속도 속성이 있는 것은 아님
  - optional 키프레임 시퀀스

![image](https://user-images.githubusercontent.com/20410193/173630608-589782dc-cc97-4dd1-b1b1-350e879b5409.png)


- 참고로 현재 Xcode의 파티클 편집기는 약간 버그가 있으므로 시작하기 전에 maximum을 0으로 변경하는 것이 좋음. 그렇지 않으면 아무 것도 표시되지 않을 수 있음

# 출처

- https://developer.apple.com/documentation/spritekit/skemitternode

- https://developer.apple.com/documentation/spritekit/skemitternode/creating_particle_effects
- https://www.hackingwithswift.com/read/11/7/special-effects-skemitternode
