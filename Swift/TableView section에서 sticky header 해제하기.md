# TableView section에서 sticky header 해제하기

- tableView에 section header 설정하면 기본적으로 sticky header

- 만약 스크롤 할때 같이 밀리는 즉 sticky header 효과를 풀고싶다면? style을 grouped 로 변경

- 근데 문제는 grouped 로 바꾸면 기본적으로 section마다 header랑 footer 가 들어감. interface builder 에서 아무리 크기를 줄여도 1

- 그래서 `tableView(_:heightForFooterInSection:)` 함수를 사용해야한다. 이때 0을 return 하면 안되고 `leastNormalMagnitude` 를 리턴해야한다. 

- 이유는 0을 리턴하면 defualt를 사용한다는 뜻이기 때문. (인터페이스 빌더에서 설정한거)

  ```swift
      func tableView(_ tableView: UITableView, heightForFooterInSection section: Int) -> CGFloat {
          return .leastNormalMagnitude //Remove space between sections
      }
  ```

- 그런데 section 사이의 space 는 사라졌어도 전체 tableView의 footerView가 남아있는 경우가 있다. 이때는 `tableFooterView` 설정을 높이 최소로해서 따로해야한다 (뷰를 nil 설정해도 해도 안되더라) 

  ```swift
  // Remove space at bottom of tableView.
  self.tableView.tableFooterView = 
  UIView(frame: CGRect(origin: .zero,
                       size: CGSize(width:CGFloat.leastNormalMagnitude,
                                    height: CGFloat.leastNormalMagnitude)))
  ```

# 참고

https://stackoverflow.com/questions/17699831/how-to-change-height-of-grouped-uitableview-header  

https://stackoverflow.com/questions/15389190/set-height-to-0-for-header-view-of-uitableview

