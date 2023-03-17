# UICollectionViewListCell custom 하기

우선  **content configuration** 과 **content view** 가 무엇인지 알고, custom cell 과의 상관관계를 알아야 함

### content view

- UIContentView protocol 을 conform 하는 UIView 의 subclass. 
- protocol 에 따라 `var configuration: UIContentConfiguration` 을 필수로 선언해야함.
- custom cell 의 layout 과 모양을 정의.
- 제공된 content configuration 에 따라서 대응하는 data 와 모양을 나타낼 필요가 있음.

### content configuration

- UIContentConfiguration protocol 을 conform 하는 content view 의 view model.
- protocol 에 따라 `func makeContentView() -> UIView & UIContentView` 와 `func updated(for state: UIConfigurationState) -> Self` 를 필수로 선언해야함
- custom cell 을 위한 content view 인스턴스를 생성하는 역할도 함. (`makeContentView()` 메서드 안에서)
- 따라서 content view 와 custom cell 의 가교 역할로 이해할 수 있음

### custom cell

- UICollectionViewListCell 의 subclass
- selected, highlighted, disabled, 등의 state 에 따라 적절하게 configure 된 content configuration object 를 생성하고, 자신의 contentConfiguration 에 대입
- `func updateConfiguration(using state: UICellConfigurationState)` 를 override 해서 이를 달성할 수 있는데, 이 메서드는 cell 의 state 가 변경될때마다 trigger

요약하자면, 

1. custom cell 은 content configuration 을 만들고 자신에게 대입
2. Content configuration 은 custom cell 을 위한 content view 를 만듦
3. content view 는 content configuration 에게 제공받은 data 로 display


<img width="986" alt="image" src="https://user-images.githubusercontent.com/20410193/225966590-d515f7d5-2052-4f17-9f7b-6e649418c04d.png">

## 출처 

- https://swiftsenpai.com/development/uicollectionview-list-custom-cell/
- https://velog.io/@aurora_97/Custom-CollectionViewListCell

