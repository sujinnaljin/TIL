# UICollectionViewDiffableDataSource 와 UICollectionViewCompositionalLayout

Sample Code 사이트를 보면서 필요했던 개념들 정리

- [Implementing Modern Collection Views](https://developer.apple.com/documentation/uikit/views_and_controls/collection_views/implementing_modern_collection_views) - iOS 14.0+
- [Updating Collection Views Using Diffable Data Sources](https://developer.apple.com/documentation/uikit/views_and_controls/collection_views/updating_collection_views_using_diffable_data_sources) - iOS 15.0+
- [Building High-Performance Lists and Collection Views](https://developer.apple.com/documentation/uikit/uiimage/building_high-performance_lists_and_collection_views) - iOS 15.0+


# View

## UICollectionView

### Creating cells

#### UICollectionView.CellRegistration

- iOS 14 +

- 셀을 컬렉션 뷰에 등록하고 각 셀을 configuration 할 수 있도록 함.

- cell registration 을 만들고 `dequeueConfiguredReusableCell(using:for:item:)`에 넘기면 셀을 따로 register 할 필요 없음

  ```
  let cellRegistration = UICollectionView.CellRegistration<UICollectionViewListCell, Int> { cell, indexPath, item in
      
      var contentConfiguration = cell.defaultContentConfiguration()
      
      contentConfiguration.text = "\(item)"
      contentConfiguration.textProperties.color = .lightGray
      
      cell.contentConfiguration = contentConfiguration
  }
  
  dataSource = UICollectionViewDiffableDataSource<Section, Int>(collectionView: collectionView) {
      (collectionView: UICollectionView, indexPath: IndexPath, itemIdentifier: Int) -> UICollectionViewCell? in
      
      return collectionView.dequeueConfiguredReusableCell(using: cellRegistration,
                                                          for: indexPath,
                                                          item: itemIdentifier)
  }
  ```

#### UICollectionView.CellRegistration.Handler

- iOS 14 +

- cell 의 register 과 configuration 을 처리하는 closure

- UICollectionView.CellRegistration 를 생성할 때 인자로 넣음

  ```
  init(handler: UICollectionView.CellRegistration<Cell, Item>.Handler)
  ```

- typealias 임

  ```
  typealias UICollectionView.CellRegistration<Cell, Item>.Handler = (_ cell: Cell, _ indexPath: IndexPath, _ itemIdentifier: Item) -> Void
  ```

### Creating headers and footers

#### UICollectionView.SupplementaryRegistration

- iOS 14.0+

- Supplementary Registration을 사용해서 헤더 및 푸터와 같은 Supplementary 뷰를 컬렉션 뷰에 등록 및 구성 함.

- SupplementaryRegistration 을 만들고 dequeueConfiguredReusableSupplementary 에 넘기면 셀을 따로 register 할 필요 없음

  ```
  let headerRegistration = UICollectionView.SupplementaryRegistration
      <HeaderView>(elementKind: "Header") {
      supplementaryView, string, indexPath in
      supplementaryView.label.text = "\(string) for section \(indexPath.section)"
      supplementaryView.backgroundColor = .lightGray
  }
  dataSource.supplementaryViewProvider = { supplementaryView, elementKind, indexPath in
      return collectionView.dequeueConfiguredReusableSupplementary(using: headerRegistration,
                                                                   for: indexPath)
  }
  ```

#### UICollectionView.SupplementaryRegistration.Handler

- iOS 14 +

- Supplementary 뷰 등록 및 구성을 처리하는 클로저

- typealias 임

  ```
  typealias UICollectionView.SupplementaryRegistration<Supplementary>.Handler = (_ supplementaryView: Supplementary, _ elementKind: String, _ indexPath: IndexPath) -> Void
  ```


# Data

## UICollectionViewDiffableDataSource

- iOS 13 +

- 데이터를 관리하고 collection view 에 셀을 제공하는 데 사용. 즉 **dataSource 정의 및 cell 모양 정의**

- 기존의 UICollectionViewDataSource 를 conform 하는 data source 는 indices 와 index paths 를 사용했음.  이는 section 및 및 item의 위치를 나타내는데, collectionview 의 내용을 추가, 제거 및 재정렬할 때 data source 가 변경될 수 있기 때문에 불안정함.

  하지만 Diffable Data Source 는 section 및 item 의 identifier 목록을 저장함. 이 목록은 collection view에 포함된 각 section 및 item 의 ID를 나타냄. 이 identifier 들은 안정적이고, 변하지 않음.

  따라서 identifier 를 사용하여 diffable data source 는 컬렉션 뷰 내에서 해당 위치에 대한 지식 없이 section 또는 item 을 참조할 수 있음 

  identifier는 hashable & equatable 하기 때문에,  Diffable Data Source  현재 스냅샷과 다른 스냅샷 간의 차이를 결정할 수 있음

- UICollectionViewDataSource protocol 을 conform 하고, 프로토콜의 모든 메서드에 대한 구현을 제공

### Creating a diffable data source

#### UICollectionViewDiffableDataSource.CellProvider

- iOS 7 +

- diffable data source 에서 collection view 에 대한 cell 을 configure 하고, 반환하는 closure

- UICollectionViewDiffableDataSource 를 생성할때 두번째 인자로 넣음

  ```
  UICollectionViewDiffableDataSource.init(collectionView:, cellProvider:)
  ```

- typealias 임

  ```
  typealias UICollectionViewDiffableDataSource<SectionIdentifierType, ItemIdentifierType>.CellProvider = (_ collectionView: UICollectionView, _ indexPath: IndexPath, _ itemIdentifier: ItemIdentifierType) -> UICollectionViewCell?
  ```

### Creating supplementary views

#### UICollectionViewDiffableDataSource.SupplementaryViewProvider

- iOS 7 +

- DiffableDataSource 로 부터 헤더 또는 푸터와 같은 컬렉션 뷰의 supplementary view 를 구성하고 반환하는 closure

- Typealias 임

  ```
  typealias UICollectionViewDiffableDataSource<SectionIdentifierType, ItemIdentifierType>.SupplementaryViewProvider = (_ collectionView: UICollectionView, _ elementKind: String, _ indexPath: IndexPath) -> UICollectionReusableView?
  ```

### Updating data

#### snapshot()

- iOS 13 +
- 컬렉션 뷰에서 데이터의 현재 상태를 반환함
- 섹션 및 아이템 식별자가 UI 에 나타나는 순서대로 포함되어 있음

#### apply(_:animatingDifferences:completion:)

- iOS 13 + 
- 선택적으로 UI 변경 내용을 애니메이션화하고,  completion 핸들러를 실행하여 스냅샷의 데이터 상태를 반영하도록 UI를 업데이트

#### apply(_:animatingDifferences:)

- iOS 15 +
- 스냅샷의 데이터 상태를 반영하도록 UI를 업데이트하고 선택적으로 UI 변경 내용을 애니메이션 함

#### applySnapshotUsingReloadData(_:)

- iOS 15 +
- diff 를 계산하거나 변경 내용을 애니메이션화하지 않고 스냅샷의 데이터 상태를 반영하도록 UI를 재설정
- 시스템은 진행 중인 아이템 애니메이션을 중단하고 컬렉션 뷰의 콘텐츠를 즉시 다시 로드 함.
- 인자로 받는 snapshot 은 콜렉션 뷰에서 데이터의 새 상태를 반영하는 스냅샷
- 백그라운드 큐에서 이 메서드를 안전하게 호출할 수 있지만, 앱에서 일관되게 호출해야 함. 항상 메인 큐 또는 백그라운드 큐에서만 이 메서드를 호출해야함

### Supporting reordering

#### UICollectionViewDiffableDataSource.ReorderingHandlers

- iOS 14.0+
- 아이템을 재정렬하는 핸들러
- canReorderItem, willReorder, didReorder 에 대한 클로저를 지정할 수 있음

#### NSDiffableDataSourceTransaction

- iOS 14 +
- 뷰에서 아이템 순서를 변경한 후의 변경 사항을 설명하는 트랜잭션
- 접근할 수 있는 프로퍼티로는 ` sectionTransections`, 트렌젝션이 발생하기 전의 `initialSnapshot` , 트랜젝션 발생 후의 `finalSnapshot` , initial 과 final 사이의 삽입 / 삭제의 변화를 나타내는 `difference` 가 있음

### Supporting expanding and collapsing

#### UICollectionViewDiffableDataSource.SectionSnapshotHandlers

- iOS 14.0+
- 아이템을 expanding 및 collapsing 할 수 있게하는 핸들러

## NSDiffableDataSourceSnapshot

- iOS 13 +

- **특정 시점**의 뷰에서 **데이터 상태**를 나타냄

- Diffable data source 는 스냅샷을 사용하여 컬렉션 뷰 및 테이블 뷰에 대한 데이터를 제공함.  뷰에 표시되는 데이터의 초기 상태를 설정하고, 데이터의 변경 사항을 반영함.

  스냅샷의 데이터는 사용자가 결정한 순서대로 표시할 section과 item 으로 구성

- 아래와 같이 스냅샷을 생성하고 DiffableDataSource 에 적용해서 데이터의 현재 상태를 생성하고 UI 에 데이터를 표시함

  ```
  var snapshot = NSDiffableDataSourceSnapshot<Int, UUID>()        
  
  // Populate the snapshot.
  snapshot.appendSections([0])
  snapshot.appendItems([UUID(), UUID(), UUID()])
  
  // Apply the snapshot.
  dataSource.apply(snapshot, animatingDifferences: true)
  ```

### Creating a snapshot

#### appendItems(withIdentifiers:)

- snapshot 의 **마지막 section** 에 특정 identifier 로 아이템을 추가함

### Reloading data

#### reconfigureItems(_:)

- iOS 15 +

- 스냅샷에 지정한 아이템들의 데이터를 업데이트하고, **아이템들의 기존 셀을 유지**함
- 새로운 셀로 바꾸지 않고 기존(사전 추출 포함) 셀의 내용을 업데이트하려면 `reloadItems(_:)` 대신 이 메소드를 사용함. 최적의 성능을 위해 기존 셀을 새 셀로 대체해야 하는 명시적인 필요가 없는 경우 항목을 다시 로드하는 대신 항목을 재구성하도록 선택.
- cell provider 는 제공된 indexPath에 대해 동일한 타입의 셀을 dequeue 하고, 동일한 기존 셀을 반환해야 함. 이 메서드는 기존 셀을 재구성하므로, 컬렉션 뷰 또는 테이블 뷰 는 대기열에 있는 각 셀에 대해 prepareForReuse 를 호출하지 않음
- indexPath에 대해 다른 타입의 셀을 반환해야 하는 경우에는 `reloadItems(_:)` 를 사용
- 만약 UICollectionViewDataSource 나 UITableViewDataSource 에 대한 custom implementation 을 사용한다면,  `reconfigureItems(at:)` 나 `reconfigureRows(at:)` 를 사용

#### reloadItems(_:)

- iOS 13 +
- 스냅샷의 지정된 아이템들에 대해 데이터를 다시 로드함
- 인자로는 snapshot 에서 reload 되어야할 item 들의 identifier 가 들어감

## NSDiffableDataSourceSectionSnapshot

- iOS 14 +
- **특정 시점**의 컬렉션 뷰 또는 테이블 뷰의 **단일 section** 에 대한 **데이터 상태**를 나타냄. 
- 섹션 스냅샷은 전체 뷰에 있는 데이터를 나타내는 NSDiffableDataSourceSnapshot 과 함께 또는 대신 사용할 수 있음. 
- section snapshot 에서 append(_:to:) 를 통해 특정 아이템의 child 로 아이템들을 지정할 수 있음. 
- expand, collpase 등 가능

# Cells

## UICollectionViewListCell

- iOS 14.0+

- list feature 와 기본 스타일을 제공하는 collection view 셀

- list cell 은 list 에 나타날 수 있는 개별 항목을 나타냄. 들여쓰기를 지원하고, UICellAccessory 같은 셀 액세서리를 추가하거나 셀과 사용자 상호 작용을 지원하는 기능을 내장함

- 모든 layout 타입에서 list cell 을 사용할 수 있음. 

- list 내에서 list cell 을 사용하면 셀에 대한 list 별 추가 동작을 사용할 수 있음. 예를 들어, list section 또는 layout 에서 list cell 간의 separator 정렬을 정의하고 각 셀의 leading 및 trailing 에 대한 스와이프 작업을 구성할 수 있음. 

  `NSCollectionLayoutSection.list(using:layoutEnvironment:)` 를 사용해서 개별 list section 을 만들거나, `UICollectionViewCompositionalLayout.list(using:)` 를 사용 해서 전체 list 레이아웃을 만들 수 있음.

- list cell 의 defaultContentConfiguration() 을 사용해서 기본 스타일이 미리 구성된 list content configuration 을 가져올 수 있음. 기본 configuration 을 가져온 후 속성을 customize 한 후 cell 에 다시 할당함

  ```
  var content = cell.defaultContentConfiguration()
  
  // Configure content.
  content.image = UIImage(systemName: "star")
  content.text = "Favorites"
  
  // Customize appearance.
  content.imageProperties.tintColor = .purple
  
  cell.contentConfiguration = content
  ```

# Layouts

## Essentials

### UICollectionViewCompositionalLayout

- iOS 13.0+

- **컬렉션 뷰**에 대한 **레이아웃 정보**를 생성하기 위한 abstract base class 

- **compositinoal 하게 레이아웃**을 쉽게 적용하기 위해서 등장. 컨텐츠에 대한 모든 종류의 시각적 배열을 구축할 수 있음.

- UICollectionViewLayout의 서브클래스

- collectionView의 레이아웃을 이루는 객체들 Item, Group, Section에 각각 레이아웃 정보를 주고, 그 객체들을 합쳐서 구성

  - NSCollectionLayout**Item** : 하나의 Item (Cell)
  - NSCollectionLayout**Group **: Item의 집합
  - NSCollectionLayout**Section** :  여러 Group의 집합

- NSCollectionLayout**Size** 생성 > Item, Group, Section 객체의 생성자에 적용

  ```
  let itemSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(0.2),
                                       heightDimension: .fractionalHeight(1.0))
  let item = NSCollectionLayoutItem(layoutSize: itemSize)
  
  let groupSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(1.0),
                                        heightDimension: .fractionalWidth(0.2))
  let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize,
                                                   subitems: [item])
  
  let section = NSCollectionLayoutSection(group: group)
  
  let layout = UICollectionViewCompositionalLayout(section: section)
  UICollectionView(frame: view.bounds, collectionViewLayout: layout)
  ```

#### UICollectionLayoutListConfiguration

- iOS 14.0+

- list 레이아웃을 만들기 위한 configuration. appearance, background, separator, header mode, footer mode, swipe 동작 등을 설정 할 수 있음

- compositional 레이아웃 (UICollectionViewCompositionLayout) 에 대한 list section 을 만들거나, list section 만 포함하는 레이아웃을 만들 수 있음

- 아래처럼 사용 됨

  ```
  let configuration = UICollectionLayoutListConfiguration(appearance: .sidebar)
  let layout = UICollectionViewCompositionalLayout.list(using: configuration)
  ```

- 섹션마다 다른 List Configuration을 구현하려면,  compositional layout 의  section provider 를 사용

- 특정 appearance 와 함께 list layout configuration 를 생성함

#### UICollectionLayoutListConfiguration.Appearance

- iOS 14.0+
- plain, grouped, insetGrouped, sidebar, sidebarPlain 등 list 의 모양을 설명하는 상수

#### UICollectionLayoutListConfiguration.HeaderMode

- iOS 14.0+
- list 에 사용할 헤더의 타입. 
- 디폴트 값은 none 이고, firstItemInSection, supplementray 가 있음.

#### UICollectionLayoutListConfiguration.FooterMode

- iOS 14.0+
- list 에 사용할 헤더의 타입. 
- 디폴트 값은 none 이고, supplementray 가 있음.

#### UIListSeparatorConfiguration

- iOS 14.5+

- list 섹션의 separator 모양을 제어하는 configuration

- 보통 아래처럼 사용되지만, 아이템 별로 separator 의 모양을 제어하기 위해서는`UICollectionLayoutListConfiguration` 의 `itemSeparatorHandler` 를 사용

  ```
  var listConfig = UICollectionLayoutListConfiguration(appearance: .plain)
  listConfig.separatorConfiguration.color = .tertiarySystemFill
  let layout = UICollectionViewCompositionalLayout.list(using: listConfig)
  ```

- visibility, inset, color 등 조정 가능

##### UIListSeparatorConfiguration.Visibility

- automatic, hidden, visible 등 list separator 의 Visibility 를 설명하는 상수

#### UICollectionLayoutListConfiguration.ItemSeparatorHandler

- iOS 14.5+

- 리스트 separator 모양을 세부적으로 제어할 수 있는 클로저

- typealais 임. 

  두번째 인자로 들어오는 sectionSeparatorConfiguration 는 해당 indexPath 의 cell 에 대한 separator configuration 으로, 현재 item 의 상태에 따른 separator 의 visibility, insets 등의 정보를 포함함.

  ```
  typealias UICollectionLayoutListConfiguration.ItemSeparatorHandler = (_ itemIndexPath: IndexPath, _ sectionSeparatorConfiguration: UIListSeparatorConfiguration) -> UIListSeparatorConfiguration
  ```

#### UICollectionLayoutListConfiguration.HeaderMode

- iOS 14.0+
- none, supplementary, firstItemInSection 등 list 의 header mode 를 나타내는 상수 (supplementary 는 header 를 보여주기 위해 supplementary view 를 사용하겠다는 것)

#### UICollectionLayoutListConfiguration.FooterMode

- iOS 14.0+
- none, supplementary 등 list 의 footer mode 를 나타내는 상수

#### UICollectionLayoutListConfiguration.SwipeActionsConfigurationProvider

- 셀에 대한 스와이프 동작을 구성하는 클로저

- typealias 임. 

  ```
  typealias UICollectionLayoutListConfiguration.SwipeActionsConfigurationProvider = (_ indexPath: IndexPath) -> UISwipeActionsConfiguration?
  ```

##  Components

### NSCollectionLayoutItem

- iOS 13.0+
- 컬렉션 뷰 **레이아웃**의 가장 **기본 component**
- 아이템은 컬렉션 뷰에서 개별 콘텐츠의 **크기, 간격 및 정렬 방법**에 대한 청사진.
- init 할때 layoutSize 를 인자로 넣어 지정하고, 추후 edgeSpacing, contentInsets 설정 가능. contentInsets 은 edgeSpacing 이 적용된 후에 적용됨.
- 일반적으로 아이템은 셀이지만, 헤더나 푸터 같은 supplementary 뷰일 수 있음

### NSCollectionLayoutGroup

- iOS 13.0+

- 아이템의 집합으로, 아이템이 서로 어떻게 배치되는지 결정함. 가로, 세로 또는 custom arrangement 로 배치할 수 있음. 

  아이템이 서로 관련되어 렌더링되는 방법에 대한 규칙을 결정하지만, 그 자체는 내용을 렌더링하지 않음

- : horizontal 라인으로 

  ```
  class func horizontal(
      layoutSize: NSCollectionLayoutSize,
      subitems: [NSCollectionLayoutItem]
  ) -> Self
  ```

- 아래 같은 타입 메소드를 통해 horizontal 혹은 vertical 로 배열된 아이템 array 를 포함하는 지정된 크기의 그룹을 만들 수 있음. custom 도 만들 수 있음

  - `class func horizontal(layoutSize: NSCollectionLayoutSize, subitems: [NSCollectionLayoutItem]) -> Self`
  - `class func horizontal(layoutSize: NSCollectionLayoutSize, repeatingSubitem: NSCollectionLayoutItem, count: Int) -> Self`
  - `class func vertical(layoutSize: NSCollectionLayoutSize, subitems: [NSCollectionLayoutItem]) -> Self`
  - `class func vertical(layoutSize: NSCollectionLayoutSize, repeatingSubitem: NSCollectionLayoutItem, count: Int) -> Self`

### NSCollectionLayoutSection

- iOS 13.0+
- 그룹의 집합
- scroll 동작 지정, section paging 지정, additional view 구성, 아이템 렌더링 등을 할 수 있음
- 컬렉션 뷰 레이아웃에는 하나 이상의 섹션이 있음. 각 섹션은 다른 섹션과 구별하기 위해 고유한 background, 헤더 및 푸터를 가질 수 있음
- 각 섹션은 컬렉션 뷰의 다른 섹션과 동일한 레이아웃 또는 다른 레이아웃을 가질 수 있음. 섹션의 레이아웃은 섹션을 만드는 데 사용되는 그룹(NScollectionLayoutGroup)의 속성에 따라 결정됨.

#### list(using:layoutEnvironment:)

- iOS 14.0+

- 타입 메소드 

- 지정된 list configuration 및 layout environment을 사용하여 list section 을 만듦

  ```
  @MainActor static func list(
      using configuration: UICollectionLayoutListConfiguration,
      layoutEnvironment: NSCollectionLayoutEnvironment
  ) -> NSCollectionLayoutSection
  ```

#### UIContentInsetsReference

- iOS 14.0+
- automatic, none, safeArea, layoutMargins, readableContent 등 content insets 의 reference point 를 설명하는 상수

## Size and spacing

### NSCollectionLayoutDimension

- iOS 13 +

- 컬렉션 뷰에서 아이템의 너비 또는 높이를 나타내는 개별 치수(dimension)

- absolute, estimated, fractionalHeight, fractionalWidth 를 이용해 지정 가능함.

  ```
  let absoluteSize = NSCollectionLayoutSize(widthDimension: .absolute(44),
                                           heightDimension: .absolute(44))
  let estimatedSize = NSCollectionLayoutSize(widthDimension: .estimated(200),
                                            heightDimension: .estimated(100))                                         
  let fractionalSize = NSCollectionLayoutSize(widthDimension: .fractionalWidth(0.2),
                                             heightDimension: .fractionalWidth(0.2))
  ```

  - .absolute: 고정된 크기일 때 사용.
  - .estimated: 크기가 변동될 수 있는 경우에 사용. (데이터가 로드되거나 시스템 폰트가 변경되었을때 등). 최종 크기는 컨텐츠가 렌더링되었을때 결정
  - fractionalWidth, .fractionalHeight: 부모 컴포넌트 크기에 비례하여 크기를 설정할 때 사용.

- isAbsolute, isEstimated 등의 인스턴스 프로퍼티를 통해 dimension type 을 알 수 있음

### NSCollectionLayoutSize

- iOS 13 +

- collection view 아이템의 높이와 너비

- `init(widthDimension:heightDimension:)` 처럼 인자로 NSCollectionLayoutDimension 을 넣어서 생성

### NSCollectionLayoutSpacing

- iOS 13.0+

- 컬렉션 뷰에서 아이템 사이 또는 아이템 주위의 공간을 정의

- 아래와 같이 fixed 혹은 flexible 사용 가능

  ```
  group.interItemSpacing = .fixed(200.0)
  group.interItemSpacing = .flexible(200.0)
  ```

  - fixed: 고정된 간격
  - flexible: 최소 간격 (증가 가능)

### NSCollectionLayoutEdgeSpacing

- iOS 13.0+

- 아이템의 가장자리 주변에 추가된 간격

- leading, top, trailing, bottom 에 대한 값을 지정할 수 있음. 여기에 들어가는 타입은 NSCollectionLayoutSpacing 임.

  ![Two diagrams that show the result of edge spacing applied to a group of items. The first diagram shows a group of three square items in a row, each item measuring 20 by 20 points. The second diagram shows a trailing edge spacing of 2 points applied to each item. Each item remains the same size, but moves 2 points if it's on the trailing edge of the previous item in the group.](https://docs-assets.developer.apple.com/published/b161480b4d/NSCollectionLayoutItem-edgeSpacing@2x.png)



### NSCollectionLayoutContainer

- iOS 13.0+
- 레이아웃 컨테이너의 size와 content insets에 대한 정보를 제공하는 데 사용되는 프로토콜
- section provider 에서 layout environment (NSCollectionLayoutEnvironment) 의 container 프로퍼티를 통해 container 의 size 나 content insets 과 같은 layout 정보를 가져올 수 있음.
- 그중 effectiveContentSize 프로퍼티는 content insets 이 적용된 이후의 container size

## Configuration

### UICollectionViewCompositionalLayoutConfiguration

- iOS 13.0+

- 스크롤 방향, 섹션 간격 및 레이아웃의 헤더 또는 푸터를 정의할 수 있음

- 여기서 정의하는 헤더 / 푸터는 (`config.boundarySupplementaryItems`) global 헤더 푸터와 같이 전체 레이아웃과 관련된거임. NSCollectionLayoutSection 에서 정의하는 헤더 / 푸터는 (`section.boundarySupplementaryItems`) 그 섹션에만 해당하는거

- UICollectionViewCompositionalLayout 을 생성할 때 이 configuration을 전달하거나 기존 레이아웃에 configuration 프로퍼티를 설정할 수 있음. 기존 레이아웃에서 configuration 을 수정하면 시스템은 새 configuration 으로 업데이트되도록 레이아웃을 무효화함

  ```
  let config = UICollectionViewCompositionalLayoutConfiguration()
  config.interSectionSpacing = 20
  config.scrollDirection = .horizontal
  config.boundarySupplementaryItems = [header, footer]
  ```

### UICollectionViewCompositionalLayoutSectionProvider

- iOS 13.0+

- 레이아웃의 각 섹션을 생성하고 return 하는 클로저

- Section Provider 를 사용하여 다른 레이아웃의 여러 섹션이 있는 UICollectionViewCompositionalLayout 를 만들 수 있음. 현재 작성 중인 섹션의 인덱스를 추적하므로 각 섹션을 다르게 구성할 수 있음.

- typealias 임

  ```
  typealias UICollectionViewCompositionalLayoutSectionProvider = (Int, NSCollectionLayoutEnvironment) -> NSCollectionLayoutSection?
  ```

### NSCollectionLayoutEnvironment

- iOS 13.0+

- 레이아웃의 컨테이너 및 환경 특성에 대한 정보를 제공하는 데 사용되는 프로토콜

- section provider 에서는 layout environment 를 사용해서 레이아웃 container 에 대한 정보 (ex. size, content insets) 및 환경 특성 (ex. size class, 디스플레이 scale facto) 등을 가져올 수 있음.

- 아래와 같이 container 및 traitCollection 에 접근 가능

  ```
  layoutEnvironment.container.effectiveContentSize.width
  layoutEnvironment.traitCollection.userInterfaceStyle == .dark
  ```

## Interaction

### UICollectionLayoutSectionOrthogonalScrollingBehavior

- iOS 13.0+

- 레이아웃 섹션의 스크롤 동작

- 기본적으로 각 섹션은 레이아웃 configuration 의 scrollDirection 속성으로 정의된 레이아웃의 주 축을 따라 내용을 레이아웃함. 다만 섹션의 orthogonalScrollingBehavior 를 .none 으로 설정함으로써 특정 섹션에 대해 이 동작을 변경할 수 있음.

- 이 프로퍼티를 none, continuos, continuousGroupLeadingBoundary, paging, groupPaging, groupPagingCentered 등 다른 값으로 변경 가능

  그 중 continuousGroupLeadingBoundary 는 visible 한 group 의 leading 에서 자연스럽게 스크롤이 멈추게함

## Appearance

### NSCollectionLayoutAnchor

- iOS 13.0+

- collection view의 item에 supplementary item 을 첨부하는 방법을 정의

- edges 와 offset (절대 or 상대) 를 통해 supplementary item 에 대한 위치를 나타냄

- 아래처럼 item 의 top trailing corner 에 뱃지를 부착할 수 있음. 

  ```
  let itemSize = NSCollectionLayoutSize(widthDimension: .absolute(44),
                                       heightDimension: .absolute(44))
      
  let badgeAnchor = NSCollectionLayoutAnchor(edges: [.top, .trailing],
                                  fractionalOffset: CGPoint(x: 0.3, y: -0.3))
      
  let badgeSize = NSCollectionLayoutSize(widthDimension: .absolute(20),
                                        heightDimension: .absolute(20))
      
  let badge = NSCollectionLayoutSupplementaryItem(layoutSize: badgeSize,
                                                 elementKind: "badge",
                                             containerAnchor: badgeAnchor)
      
  let item = NSCollectionLayoutItem(layoutSize: itemSize,
                            supplementaryItems: [badge])
  ```

### NSCollectionLayoutSupplementaryItem

- iOS 13.0+
- NSCollectionLayoutItem 을 상속
- 컬렉션 뷰의 아이템에 추가적인 시각적 장식을 설정하는 데 사용함. supplementary item 을 이용해서 content 에 추가 뷰를 부착할 수 있음 (Ex. 배지를 item 이나 group 주위의 프레임에 부착)
- 레이아웃 또는 해당 섹션에 대한 헤더 또는 푸터 작성하려면 NScollectionLayoutBoundarySupplementItem 를 사용
- 각 supplementary item 은 element kind 가 있어야함

### NSCollectionLayoutBoundarySupplementaryItem

- iOS 13.0+
- NSCollectionLayoutSection 의 boundarySupplementaryItems 에 세팅
- supplementary item 의 특정한 타입으로, NSCollectionLayoutSupplementaryItem 을 상속함
- 컬렉션 뷰 섹션, 혹은 컬렉션 뷰 전체의 헤더 혹은 푸더를 추가하는데 사용 됨
- 생성할 때 size, element kind, alignment, absolute offset 등을 지정할 수 있음
- pinToVisibleBounds 를 통해 visibliy boundary 의 top 또는 bottom 에 고정되는지 설정할 수 있음

### NSCollectionLayoutDecorationItem

- iOS 13.0+

- 컬렉션 뷰의 섹션에 배경을 추가하는 데 사용됨

- NSCollectionLayoutSection 의 decorationItems 에 세팅하고, `register(_:forDecorationViewOfKind:)` 를 이용해 layout 과 함께 background view 등록

  ```
  let sectionBackground = NSCollectionLayoutDecorationItem.background(
          elementKind: ElementKind.background)
  
  section.decorationItems = [sectionBackground]
  
  let layout = UICollectionViewCompositionalLayout(section: section)
  layout.register(
      SectionBackgroundDecorationView.self,
      forDecorationViewOfKind: ElementKind.background)
  return layout
  ```

## Layout updates

### NSCollectionLayoutVisibleItem

- iOS 13.0+
- section의 bounds 내에서 현재 visible 한 item (cell, supplementary view, decoration 같은..)
- visibleItemsInvalidationHandler 프로퍼티에 저장된 `NSCollectionLayoutSectionVisibleItemsInvalidationHandler` 를 통해 섹션의 visible 한 아이템들을 접근할 수 있음

### NSCollectionLayoutSectionVisibleItemsInvalidationHandler

- iOS 13.0+

- 섹션의 아이템이 표시되기 직전에 수정할 수 있도록 각 레이아웃 cycle 전에 호출되는 클로저

- NSCollectionLayoutSection 의 visibleItemsInvalidationHandler 에 세팅

  이를 통해 해당 섹션의 범위 내에서 현재 표시되는 아이템에 대해 사용자 지정 애니메이션을 수행할 수 있음. 아이템 추가 또는 제거, 섹션 스크롤 또는 디바이스 회전과 같은 변경 사항으로 인해 해당 섹션에서 애니메이션이 발생할 때마다 각 레이아웃 주기 전에 핸들러가 호출됨

  ```
  section.visibleItemsInvalidationHandler = { visibleItems, scrollOffset, layoutEnvironment in
      // Perform animations on the visible items.
  }
  ```

- typealias 임

  ```
  typealias NSCollectionLayoutSectionVisibleItemsInvalidationHandler = ([NSCollectionLayoutVisibleItem], CGPoint, NSCollectionLayoutEnvironment) -> Void
  ```

# 기타

## UITraitCollection

- iOS 8.0+
- 가로 및 세로 size class, 디스플레이 크기 및 user interface idiom 과 같은 특성을 포함한 앱의 iOS 인터페이스 환경.

## UIListContentConfiguration

- iOS 14.0+

- list 기반 content view 대한 Content Configuration

- struct 임

- cell, 헤더, 푸터와 같은 list 에 나타날 수 있는 개별 요소의 스타일 및 내용을 설명

- valueCell(), subtitleCell(), plainHeader(), plainFooter() 등의 타입 메서드를 통해 다양한 view state 에한 시스템 기본 스타일을 얻을 수 있음. 

- configuration 을 content 로 채운 다음, UICollectionView 및 UITableView 의 셀, 헤더 및 푸터 또는 custom list content view (UIlistContentView)에 직접 할당함

- updated(for: state)

- UITableViewCell 인스턴스에서 `cell.defaultContentConfiguration()` 처럼 셀의 스타일을 위해 사용할 수 있음.

  원하는 요소만 바꿔둔 다음에 다시 cell 에 할당 가능

  ```
  var content = cell.defaultContentConfiguration()
  
  // Configure content.
  content.image = UIImage(systemName: "star")
  content.text = "Favorites"
  
  // Customize appearance.
  content.imageProperties.tintColor = .purple
  
  cell.contentConfiguration = content
  ```

## UIConfigurationState

- iOS 14.0+
- protocol 임. 뷰의 상태를 캡슐화하는 개체에 대한 요구 사항

## UIContentConfiguration

- iOS 14.0+
- content view 에 대한 configuration 을 제공하는 개체의 요구 사항
- 이 프로토콜은 content view 의 기본 스타일과 컨텐츠를 포함하는 content configuration object 에 대한 Blueprint를 제공
- content configuration 은 content view customization 을 위해 지원되는 모든 속성 및 동작을 캡슐화함. 
- configuration 을 사용하여 content view 를 만듦

## NSDirectionalRectEdge

- iOS 13.0+
- top, trailing 등 user interface 레이아웃 방향을 고려하여 edge 또는 edge set 을 지정하는 상수

## NSDirectionalEdgeInsets

- iOS 11.0 +

- 뷰에 대한 inset

- leading, top, trailing, bottom 에 대한 값을 지정할 수 있음. 여기에 들어가는 타입은 CGFloat 임.

- 이 값은 dimension 이 estimated 로 적용된 경우 무시됨

  ![Two diagrams that show the result of content insets applied to a group of items. The first diagram shows a group of three square items in a row, each item measuring 20 by 20 points. The second diagram shows content insets of 2 applied to each edge of each item, resulting in each item becoming 16 by 16 points. The group remains the same size.](https://docs-assets.developer.apple.com/published/e3bdac910b/NSCollectionLayoutItem-contentInsets@2x.png)


