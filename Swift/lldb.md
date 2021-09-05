# LLDB (Low Level Debugger)
- LLVM 프론트엔드에 대응하는 디버거
- LLVM은 Apple 에서 진행한 Compiler에 필요한 Toolchain 개발 프로젝트로 컴포넌트들의 재사용성을 중시해서, 모듈화가 잘 되어있다는 특징이 있음. 이렇게 모듈화 되어있는 컴포넌트들을 이용해 진행된 주요 서브 프로젝트들로는 LLVM Core, Clang, libc++, LLDB 등이 존재
- LLDB는 LLVM의 Debugger Component를 개발하는 서브 프로젝트. LLVM 프로젝트를 통해 개발된 Clang Expression Parser, LLVM Diassembler 등 Low-Level 컨트롤이 가능한 모듈들로 이루어져 있어, 기계어에 가까운 영역까지 디버깅 가능하다는 장점이 있음
- C, C++, Objective-C, Swift를 지원하며, 현재 Xcode의 기본 디버거로 내장되어 있음.
## 문법

```bash
(lldb) command [subcommand] -option "this is argument"
```

- Command, Subcommand, Option, Argument들로 이루어져 있고, 띄어쓰기로 구분

  - **Command와 Subcommand**는 LLDB 내 Object의 이름. (etc. *breakpoint*, *watchpoint*, *set*, *list* … ) 이들은 모두 계층화되어있어, Command에 따라 사용가능한 Subcommand 종류가 다름
  - **Option**의 경우, Command 뒤 어느 곳에든 위치 가능하며, `-`(hyphen) 로 시작
  - **Argument**에 공백이 포함 되는 경우도 있기 때문에, `""`로 묶어줄 수 있음

  **예시**

  ```bash
  (lldb) breakpoint set --file test.c --line 12
  ```

  > `breakpoint set` : breakpoint (Command)와 set (Subcommand)을 이용해 중단점 설정
  > `--file test.c` : --file option을 통해 test.c 파일의
  > `--line 12` : --line option을 통해 12번째 라인에

## 도움 받기

#### `help` 명령어

- 해당 문법으로 사용가능한 Subcommand, Option 리스트나 사용법을 보여줌

```bash
# LLDB에서 제공하는 Command가 궁금하다면,
(lldb) help

# 특정 Command의 Subcommand나, Option이 궁금하다면,
(lldb) help breakpoint
(lldb) help breakpoint set
```

#### `apropos` 명령어

- 원하는 기능의 명령어가 있는지 관련 키워드를 통해 알아볼 수 있음

```bash
# referent count를 체크할 수 있는 명령어가 있을까? 궁금하다면,
(lldb) apropos "reference count"
# 결과
# The following commands may relate to 'reference count':
#    refcount -- Inspect the reference count data for a Swift object
```

## Run

#### `run` 명령어

- 현재 프로그램을 중단하고, 새로운 Build/Run을 진행

```
(lldb) run
```

## Breakpoint

### 만들기

#### `b` (`_regexp-break`) 명령어

- 간단하게 Breakpoint 생성을 할 수 있도록 도와주는 Shorthand Command

```sh
# 특정 이름을 가진 function에서 break
(lldb) b viewDidLoad
# 현재 파일 20번째 line에서 break
(lldb) b 20
# 특정 파일 20번째 line에서 break
(lldb) b ViewController.swift:12
# 현재 파일 내 특정 text를 포함한 line에서 break
(lldb) b /stop here/
# 특정 주소값에서 break
(lldb) b 0x1234000
```

#### `breakpoint set [option]` 명령어

- `breakpoint` -> `br` 로 치환 가능
- `set` -> `s` 로 치환 가능

##### 옵션

- `–name` (`-n`) :  **특정 이름**을 가진 모든 **함수**에 break를 걸 수 있음

```shell
  # 함수 이름 이용해 break
  (lldb) breakpoint set --name viewDidLoad
```

> 👩🏻‍💻 --name을 -name 처럼 option에 - 한개만 줘도 동작하는 듯. 근데 자동완성이 안됨

- `–func-regex` (`-r`)  : 정규표현식 활용 가능

```shell
  (lldb) breakpoint set  --func-regex '^hello'
  (lldb) br s -r '^hello'
  # 'breakopint set --func-regex'는 줄여서 'rb'로 사용 가능
  (lldb) rb '^hello'
```

- `–file` (`-f`), `–line` (`-l`) : **파일의 이름**과 **line 번호**를 이용해 break 걸기

  ```shell
  # 특정 파일의 20번째 line에서 break 
  (lldb) br s --file ViewController.swift --line 20
  (lldb) br s -f ViewController.swift -l 20
  ```


- `-–contidion` (`-c`) : breakpoint에 원하는 조건 걸기. (option 뒤의 expression이 true인 경우에만 멈춤)

```shell
  # viewWillAppear 호출시, animated가 true인 경우에만 break
  (lldb) breakpoint set --name "viewWillAppear" --condition animated
  (lldb) br s -n "viewWillAppear" -c animated
  (lldb) br s -l 27 -condition 'helloWorld == "hello World"'
```

- `-command` (`-C`) : break시 원하는 lldb command를 실행 가능

```shell
  #  –auto-continue (-G) 는 command 실행 후 break에 걸린채로 있지 않고 프로그램을 자동 진행하게 해줌
  (lldb) breakpoint set -n "viewDidLoad" --command "po $arg1" -G1
  (lldb) br s -n "viewDidLoad" -C "po $arg1" -G1
```

- `-one-shot` (`-o`) : 브레이크 포인트가 한번만 실행되고 자동으로 사라짐

```sh
(lldb) breakpoint set --one-shot true -f ViewController.swift -l 90
(lldb) br s -o -f ViewController.swift -l 91
```

- 물론 위에서 설명한 옵션들 중 일부는 UI에서 설정할 수도 있음 (-condition, -command, -auto-continue 등)

![image](https://user-images.githubusercontent.com/20410193/132126800-61acef63-309f-4060-82df-36e1cd27cfc9.png)


🤯🤯🤯🤯 **여기서 Action 쪽이 오져버리는게 뭐냐면 만약 내가 `tableView.delegate = self` 코드를 추가 안해서 에러난거 같다? 싶으면 그 코드 추가한 후에 다시 확인을 위해 run 해야 했음 -> 근데 추가해야하는 부분에 브포 걸고 Action 으로 추가해준뒤 해당 코드 들어오게 trigger 한다? 그리고 잘 된다? 그럼 굳이 run 안해도 되는거;;; ㅁㅊㄷ ㅁㅊㅇ;;;;**

```sh
(lldb) breakpoint set -line 64 -command "expression textView.delegate = self" -auto-continue 1
(lldb) br s -l 64 -C "expression textView.delegate = self" -G1
```

#### 참고 - `watchpoint` 명령어

- breakpoint와 유사하지만 다음에 해당 값이 변경될 때 디버거를 일시 중지

```sh
# 변수 이름으로 watchpoint 설정
(lldb) watchpoint set variable my_var
(lldb) wa s v my_var

# 메모리 주소에 watchpoint 설정
(lldb) watchpoint set expression -- my_ptr
(lldb) wa s e -- my_ptr
```

- 하단 variable views에서 관찰하고 싶은 속성 우클릭하면 Watch "myProperty" 뜨는데 이걸로도 설정 가능

![image](https://user-images.githubusercontent.com/20410193/132126735-660a827c-d860-42f9-839e-030fc3eae0c4.png)

### list 확인

#### `breakpoint list` 명령어

- 현재 프로그램에 생성되어있는 Breakpoint의 목록 확인 가능

```sh
  # breakpoint 목록 전체 출력
  (lldb) breakpoint list
  (lldb) br list
  
  # 특정 id를 가진 breakpoint의 정보만 출력
  (lldb) br list 1
  
  /*
  Current breakpoints:
1: file = '/Users/kangsujin/Desktop/sample/sample/ViewController.swift', line = 13, exact_match = 0, locations = 1, resolved = 1, hit count = 1

  1.1: where = sample`sample.ViewController.viewDidLoad() -> () + 20 at ViewController.swift:13:9, address = 0x00000001024df674, resolved, hit count = 1 
  */
```

- 해당 목록 정보에는 Breakpoint의 id와 이름, hit-count 정보, enable 여부, source 상의 위치, 주소값 등의 정보가 포함

  > hit count?
  >
  > 프로그램 실행 중 활성 상태인 Breakpoint 지점이 실행되면, Debugger는 hit count를 1씩 늘려가며 기록.
  > 하지만 Breakpoint가 걸려있다 하더라도, disable 상태이면 count되지 않음.

##### 옵션 

- `–brief` (`-b`) option을 통해 간단한 내용을 확인해 볼 수도 있음

```shell
  # breakpoint 목록 간단하게 출력
  (lldb) br list -b
  
  /*
  Current breakpoints:
1: file = '/Users/kangsujin/Desktop/sample/sample/ViewController.swift', line = 13, exact_match = 0, locations = 1, resolved = 1, hit count = 1
2: file = '/Users/kangsujin/Desktop/sample/sample/ViewController.swift', line = 24, exact_match = 0, locations = 1, resolved = 1, hit count = 3
  */
```

### 삭제

#### `breakpoint delete` 명령어

```shell
  # breakpoint 전체 삭제
  (lldb) breakpoint delete
  (lldb) br de
  # 특정 breakpoint 삭제 (br list 로 확인하면 나오는 breakpoint id)
  (lldb) br de 1
```

### 비활성화

#### `breakpoint disable` 명령어

```shell
  # breakpoint 전체 비할성화
  (lldb) breakpoint disable
  (lldb) br di
  # 특정 breakpoint 비활성화
  (lldb) br di 4
```

- 만약 `br di 4` 로 4번 breakpoint disable 시키고 `br list -b` 로 찍어보면 아래 같이 `Options: disabled` 뜸

```
4: file = '/Users/kangsujin/Desktop/sample/sample/ViewController.swift', line = 27, exact_match = 0, locations = 1 Options: disabled 
```

- `br di 4.1` 처럼 `br list` 호출하면 나오는 세부 정보(?)로 breakpoint disable 시킬수도 있는듯. 근데 실상 결과는 같은 듯 

```
4: file = '/Users/kangsujin/Desktop/sample/sample/ViewController.swift', line = 27, exact_match = 0, locations = 1
  4.1: where = sample`sample.ViewController.aa() -> () + 677 at ViewController.swift:27:15, address = 0x000000010612fad5, unresolved, hit count = 1  Options: disabled 
```

### 활성화

#### `breakpoint enable` 명령어

```shell
  # breakpoint 전체 활성화
  (lldb) breakpoint enable
  (lldb) br en
  # 특정 breakpoint 활성화
  (lldb) br en 4
```

## Execution Command

- Execution 명령들은 LLDB Console 상단에 위치한 네개 버튼과 같은 역할. 차례대로 Continue, Step-Over, Step-In, Step-Out 을 의미

![image](https://user-images.githubusercontent.com/20410193/132126765-a268b144-8ab4-4540-9780-0fe43b7c618b.png)

- Thread가 생겨날 때, 해당 Thread를 위한 **Stack**이 만들어지며, 해당 **Stack**에는 Frame이 들어감
- 현재 스레드의 현재 스택 프레임에 대한 빠른 개요를 얻고 싶다면 `frame info` 명령어를 통해 확인할 수 있음
- 현재 스레드에 대한 정보는 `thread info` 로, 모든 스레드 정보는 `thread list` 로 확인 가능

### Continue

#### `continue` (`c`) 명령어

- `process continue` 의 약어
- 정지된 프로그램 실행을 재개. 다음 Breakpoint가 나타날때까지 프로그램을 진행

```
  (lldb) process continue
  (lldb) continue
  (lldb) c
```

- process 명령어는 현재 플랫폼의 프로세스와 상호작용 할 수 있게 도와주는 명령어로  `process status` 같은 경우는 현재 디버거가 어디 있는지 찾을 수 있게 해줌

```
(lldb) process status
```

### Stepping Over


#### `next` (`n`) 명령어

- `thread step-over` 의 약어

- 현재 Break 걸려있는 지점에서 바로 **다음 Statement**로 이동.  (현재 선택된 Frame에서 소스 수준의 한 단계를 진행)

```
  (lldb) thread step-over
  (lldb) next
  (lldb) n
```

### Stepping In


#### `step` (`s`) 명령어

- `thread step-in` 의 약어 
- 현재 선택된 Frame에서 소스 수준의 한 단계 안으로 들어감
- 다음 Statement가 Function Call 인경우 Debugger를 해당 **함수 내부에 위치한 시작 지점**으로 이동

````
  (lldb) thread step-in
  (lldb) step
  (lldb) s
````

### Stepping Out

####`finish` 명령어

- `thread step-out`  의 약어

- 현재 진행중인 function이 return 될때까지 프로그램을 진행한 후 프로그램 Break 걸어주는 Stepping Action (현재 선택된 Frame에서 벗어남)
- Stack Memory 관점에서 Stepping Out은 Stack Frame을 Pop하는 것과 동일 (왼쪽 Debug Navigator를 잘 살펴보면 Stepping-In과 Stepping-Out 시 StackFrame이 어떻게 달라지는 지 확인 가능)

```
  (lldb) thread step-out
  (lldb) finish
```

- 단축어로 `f` 로 되지 않을까 싶었는데 그건 `frame` 명령어의 약자였다

## Evaluating Expression

- `expression` (`expr`, `ex`, `e`) 명령어는 LLDB에서 가장 유용하고 강력한 명령 중 하나로 현재 스레드에서 expression을 evaluate
- `expression` 을 사용하면 코드를 실행할 수 있을 뿐만 아니라 프로젝트를 다시 컴파일하지 않고도 기존 변수를 수정할 수 있음

### Variable 출력하기

#### `expression`  (`expr`, `ex`, `e`) 명령어

- expression 을 이용해서 특정 변수의 값을 출력할 수 있음

```sh
(lldb) e <variable> # 특정 변수의 값 출력
```

##### 옵션

- `–-depth` (`-D`) :  aggregate type 을 dump 할 때 최대 recurse depth 설정 가능 (default 는 infinity)

```sh
(lldb) e -D 1 -- self

# 출력
(sample.SecondViewController) $R9 = 0x00007f855101c8f0 {
  UIKit.UIViewController ={...}
  textView = 0x00007f8552831800{...}
  textViewMessage = ""{...}
}
```

#### `po` 명령어

- `expression -O --` 의 축약형 (or `expression -object-description --`)

  여기서 `-O` 는 object의 설명(debugDescription)을 출력하겠다는 의미. option을 사용하면 반드시 `--` 를 option과 raw input 사이에 넣어야함 (`help po` 나 `help expression` 치면 설명 나옴)

```
  (lldb) expression -O -- self
  (lldb) po self
```

- `po`가 출력하는 description은 `NSObject`의 `debugDescription` . 따라서 해당 프로퍼티를 override 하면 po 결과 customize 가능

```swift
    override var debugDescription: String {
        return "sujinnaljin의 description \(super.debugDescription)"
    }
```

```bash
(lldb) po self
# sujinnaljin의 description <sample.ViewController: 0x7fc66ed08850>
```

- struct 같이 NSObject 상속 안하는거에 커스텀 디버깅 인포 찍고싶다? 하면  `CustomDebugStringConvertible` protocol conform

```swift
struct Trip {
 var name: String
 var desinations: [String]
}

extension Trip: CustomDebugStringConvertible {
  var debugDescription: String { "Trip description" }
}
```

- 하지만 이는 top level description만 변경하기 때문에 substructure까지 변경하고 싶다하면 `CustomReflectable` protocol conform 하면 됨.  Objective-C 객체라면 description method를 implement하면 됨.

- `po`는 변수만 출력하는 것이 아니라  프롬프트에 컴파일되는 모든 내용이 argument로 전달되어 arbitary expression을 수행할 수 있음

```sh
(lldb) po cruise.name.uppercased() # 대문자 계산
(lldb) po cruise.desinations.sorted()  # 알파벳 정렬 순
```

- Swift debugging context에서 `expression [self.view recursiveDescription]` 과 같이 Objective-C 표현을 사용하면 오류가 나는데 이럴때는 Objective-C 실행을 강제하기 위한 `-l` (`-language`) 옵션 존재

- 기존 po Command에 `-l objc` option을 추가해서 Swift Context안에서도 Objective-C Expression을 사용할 수 있고 그 반대도 마찬가지 (아마?)

```
(lldb) e -l objc -O -- [self.view recursiveDescription]
```

- 하지만 위의 코드는 에러가 나는데, 이유는 Objective-C 컴파일을 위한 임시 expression context를  컨텍스트를 생성하며 Swift 프레임에서 모든 변수를 상속하지 않기 때문. 

- 이때 self.view에 backticks 을 추가해주면 되는데 이 의미는 '현재 frame 에서, backtick 안에 있는 것부터 먼저 evaluate 해서 결과에 넣은 후에 나머지를 evaluate 하는 것 ' 임.

```
(lldb) e -l objc -O -- [`self.view` recursiveDescription]
```

#### `p` 명령어 ()

- `print` 의 약자로 `expression -- ` 명령어의 축약형 (참고로 `po` 는 `expression -O --` 의 alias)

  하지만 `print` 명령어는 flag나 추가적인 argument 를 받지 않음

  ```swift
  (lldb) e -raw -- cruise.name
  (Swift.String) $R16 = {
    _guts = {
      _object = {
        _countAndFlagsBits = {
          _value = -7427134353699732757
        }
        _object = 0xa9000000000000b8
      }
    }
  }
  (lldb) p -raw -- cruise.name
  error: <EXPR>:8:2: error: cannot find 'raw' in scope
  -raw -- cruise.name
   ^~~
  
  error: <EXPR>:8:6: error: cannot find operator '--' in scope; did you mean '-= 1'?
  -raw -- cruise.name
  ```

- LLDB에서 변수를 인쇄하는 또 다른 방법으로, 객체의 설명 없이 print 하는 것이라고 생각하면 됨

- po에서 제공하는 표현과 약간 다르지만 동일한 정보를 담고 있음 (`po` 는 `debugDescription` 정보를 이용하고, `p` 나 `v` (`frame variable`) 는 lldb의 `formatters` 를 이용한다고 함)

```sh
# po cruise
(lldb) po cruise
▿ Trip
  - name : "날진호"
  ▿ desinations : 1 element
    - 0 : "Sorrento, Capri, Taormina"

# p cruise
(lldb) p cruise
(sample.Trip) $R4 = {
  name = "날진호"
  desinations = 1 value {
    [0] = "Sorrento, Capri, Taormina"
  }
}
```

- 주목할 점은 결과 값에는 \$R0과 같은 이름이 지정된다는 것. 따라서 `$R4.name` 과 같이 참조 가능

#### 참고 - `v` 명령어

- `frame variable` 의 약자 (not `expression`)
- output으로 출력되는 결과가 p 명령어와 같음. 왜냐? 출력시 p와 같은 formatter를 이용하기 때문

```swift
struct Trip {
    var name: String
    var subName: String {
        return "sub" + name
    }
    var desinations: [String]
}

(lldb) p cruise
(sample.Trip) $R2 = {
  name = "날진호"
  desinations = 1 value {
    [0] = "Sorrento, Capri, Taormina"
  }
}
(lldb) v cruise
(sample.Trip) cruise = {
  name = "날진호"
  desinations = 1 value {
    [0] = "Sorrento, Capri, Taormina"
  }
}
```

- 다만 po나 p 명령어와 달리 코드를 compile & evaluate 하지 않으므로 속도가 매우 빠름. 메모리에 있는거 바로 읽어옴
- 코드를 컴파일하지 않기 때문에 현재 스택 프레임에 있는 변수에만 액세스 가능하고, computed property는 계산 불가 (exectued 되어야하기 때문)

```sh
(lldb) v cruise.subName
error: "subName" is not a member of "(sample.Trip) cruise"
(lldb) p cruise.subName
(String) $R1 = "sub날진호"

(lldb) v cruise.name.isEmpty
error: "isEmpty" is not a member of "(Swift.String) cruise.name"
(lldb) p cruise.name.isEmpty
(Bool) $R4 = false
```

### Variable 사용하기

- Runtime에 여러 정보를 출력할 수 있을 뿐아니라 값을 변경 할 수도 있음

```sh
(lldb) expression <expression> # evaluate expression
```

```swift
(lldb) e self.view.backgroundColor = .red
(lldb) c
```

- LLDB는 내부적으로 값이 출력될때마다 local variable을 *$R~*의 형태로 만들어 저장하는데 이 값들은 해당 break context가 벗어나도 사용 가능한 값들이고,  수정해서 사용할 수도 있음


```sh
(lldb) e self.view
(lldb) e $R0!.backgroundColor = UIColor.blue # R0 은 위의 e self.view 에서 self.view가 저장된 위치
(lldb) continue # 코드 마저 진행
```

### Variable 선언하기

- `expression` Command를 이용해서 변수를 직접 선언해서 사용할 수도 있음. 단, 사용하고자 하는 변수명 앞에 `$` 문자를 붙여야 함

```sh
(lldb) expr let $someNumber = 10
(lldb) expr var $someString = "some string"
```

- 활용 예시

```sh
# 임의의 View 생성해서 얹기
(lldb) e var $view = UIView(frame: CGRect(x: 100, y: 100, width: 80, height: 80))
(lldb) e # `(lldb) expression` 명령어를 입력한 후 return키를 입력하면, Multi-line Command를 입력할 수 있음
Enter expressions, then terminate with an empty line to evaluate:
1 $view.backgroundColor = UIColor.blue
2 self.view.addSubview($view)
3 
(lldb) c
```

```sh
# 임의의 UIViewController 생성하여 NavigationViewController에 Push
(lldb) expr var $vc = UIViewController()
(lldb) expr $vc.view.backgroundColor = UIColor.red
(lldb) expr self.navigationController?.pushViewController($vc, animated: true)
```

- 변수 말고도 method 및 class 도 만들어서 사용 가능

```sh
(lldb) po 
Enter expressions, then terminate with an empty line to evaluate:
1 class $A {
2 var $b = 0
3 }
4 
(lldb) po $A()
<$A: 0x600001bc87e0>
(lldb) po $A().$b
0
```

- extension에 함수를 만들어 사용할 수도 있음

```sh
(lldb) po
Enter expressions, then terminate with an empty line to evaluate:
1 extension ViewController {
2 func $changeBgColor() {
3 self.view.backgroundColor = .red
4 }
5 }
6 
(lldb) po self.$changeBgColor()
```

- `--ignore-breakpoints` (`-i`) option 을 통해 expression 실행 중 만나는 breakpoint를 ignore 여부 선택 가능. (default는 –ignore-breakpoint true)

```sh
# 그냥 expression 함수명을 입력하면 해당 함수를 실행함. 이때 함수 안에 걸려있는 break point는 무시
(lldb) expression helloWorld()

#  실행 도중 breakpoint를 만나도 그냥 진행 (default)
(lldb) expression -ignore-breakpoints true -- helloWorld()
(lldb) ex -i 1 -- helloWorld()

#  실행 도중 breakpoint를 만나면 멈춤
(lldb) expression -ignore-breakpoints false -- helloWorld()
(lldb) ex -i 0 -- helloWorld()
```

- 객체의 주소값과 Type만을 알고있는 경우 Swift의 `unsafeBitCast(to:)` 함수를 이용해 변수로 사용할 수 있음

```sh
(lldb) e let $myObject = unsafeBitCast(0xabcdef, to: UILabel.self) # 0xabcdef에 할당된 UILabel 에 접근해 변수로 선언
(lldb) e $myObject # 변수를 이용해 해당 object의 정보 알 수 있음
(lldb) e $myObject.text = "hello world" # 변경도 가능
```

- unsafeBitCast 이용하는건 특히 debug View Hierarchy 에서 유용한 듯
- 객체 선택 한 다음에 cmd + c 하면 `((UITextView *)0x7f8746047600)` 이런식으로 선택된 object 의 casted pointer 을 줌 

```sh
# debug View Hierarchy 클릭해서 lldb 로 들어왔을 때는 기본적으로 objc context 인듯. 따라서 swift context로 만들어줌. 여기서 한번에 안해주면 아래 매 명령 앞에 e -l swift -- 붙여야함
(lldb) settings set target.language swift 
# UIkit을 import하여 LLDB Console에서 UIButton type 사용할 수 있게 함 (view hierarchy 에서 lldb 로 들어왔으니까)
(lldb) e import UIKit 
# 0xabcdef에 할당된 UIButton 에 접근해 값 변경
(lldb) e unsafeBitCast(0xabcdef, to: UIButton.self).backgroundColor = .red 
# 디버거에서 일시 중지되어 있기 때문에 core animation에서 현재 화면의 프레임 버퍼에 뷰 모듈 변경 사항을 적용하고 있지 않음. 따라서 CATransaction 의 flush 함수를 통해 core animation이 screen의 frame buffer를 업데이트하도록 함
(lldb) e CATransaction.flush() 
```

- 오토레이아웃도 컨스트레인트 잡고 비슷하게 변경 가능

```sh
(lldb) e -l swift -- unsafeBitCast(0x600003926710, NSLayoutConstraint.self).constant = 300
(lldb) e -l swift -- CATransaction.flush()
```

- 사실 viewHierarchy 에서 객체 잡고 cmd + c 하면 `((NSLayoutConstraint *)0x600003926710)` 이런 식으로 선택된 object 의 casted pointer 을 줌. debug View Hierarchy 클릭해서 lldb 로 들어왔을 때는 기본적으로 objc context 인 듯. 

```
(lldb) e [((NSLayoutConstraint *)0x600003926710) setConstant: 130]
(lldb) e -l swift -- CATransaction.flush()
```

## Alias 

- `(lldb) command alias 별명 "줄이고 싶은 Command"` 명령어를 이용하면 간단한 별명으로 줄여서 이용 가능
- 예를 들면, Swift Code 내에서 Objective-C expression을 출력하기 위해 `(lldb) expression -l objc -O --` Command를 사용하는 경우가 종종 있는데, 아래 명령어를 실행하고 난 후에는 `(lldb) pojc "[Objective-C Expression]"` 형태로 명령어 사용 가능

```
(lldb) command alias pojc expression -l objc -O --
```

- `settings set target.language swift` 나 `expr -l swift` 이런거 별칭으로 해도 좋을 듯. 아니면 `e CATransaction.flush()` 같은거.
- 하지만 Alias로 등록해두어도 별칭은 **해당 빌드 시점이 지나면, LLDB Session이 끝남과 동시에 사라지**기 때문에 Xcode를 실행시 LLDB 초기화를 위해 사용되는 **~/.lldbinit** 파일에 원하는 Command Alias를 추가해두면, 별칭을 매번 정해주지 않고 계속 사용 가능

## LLDB Script

- Python을 LLDB Script로 사용할 수 있음.

```
(lldb) script print(1 + 2)
```

- 그래서 Derek Selander나 Chisel 등을 import 하여 해당 라이브러리에 정의되어 있는 많은 기능들을 사용할 수 있음
  - [오픈소스 LLDB Script 사용해보기](https://yagom.net/courses/start-lldb/lessons/open-source-script/)
  - [[Xcode][LLDB]Debugging With Xcode, LLDB and Chisel](http://minsone.github.io/ios/mac/xcode-lldb-debugging-with-xcode-lldb-and-chisel)

## Image

#### `image` 명령어

- **Module** 내에서 나타나는 **Symbol**에 대한 자세한 정보를 알아낼 수 있는 명령어

  > **Module?**
  >
  > - Process에 Load되어 실행되는 Code를 의미
  > - 스위프트에서 Module은 메인 실행파일 뿐 아니라, Framework나 Library, Plugin등도 포함하는 개념으로 우리가 만든 Application 프로젝트를 포함해서, UIKit, AppKit 등의 Library 까지도 Module로 볼 수 있음
  >
  > **Symbol?**
  >
  > - Method, Variable, Class 등 말그대로 기계가 아닌 사람의 눈으로 알아볼 수 있는 Source Code를 이루는 작은 단위의 **Symbol**
  > - **Symbol Table**은 Compile된 Binary를 그에 맞는 Method, Variable, Class 등으로 상호 Mapping 해주는 역할 수행
  > - 또한 Binary가 Symbol로 번역되는 것을 **Symbolicate**라고 함

- Framework에 대한 Private한 정보들 (private Class, private Method 등)을 Header File에 공개되어 있는 것 이상으로 알아 볼 수 있음

- **Private API**들에 접근 가능하다면, 객체에 대한 숨겨진 Description을 확인하거나, 더 세부적인 속성을 확인할 수 있기 때문에 디버깅이 좀 더 간편해질 것

### Image List

#### `image list` 명령어

- 현재 Process에 Load 되어있는 모든 Module들의 정보를 출력

```
(lldb) image list
```

- 프로젝트 생성 이후 구현한 내용이 없는데도 무려 350개 이상의 Module이 Load 되어있음 ㅇ0ㅇ
- 각 모듈마다 표현하는 정보는 현재 Load 되어있는 Module들의 **UUID**, 실제 Memory상 Load되어있는 **주소값**, 그리고 Disk 내에서 Binary가 존재하는 **Path**

![lldb_image_list_summary](https://yagom.net/wp-content/uploads/2020/02/lldb_image_list_summary-1024x226.png)

### Image Dump

#### `image dump` 명령어

- Module의 세부적인 정보를 dumping (dump는 기억장치의 내용을 기록하는 것) 

```
(lldb) image dump
```

- 만약 UIKitCore 라이브러리의 정보를 뽑아내고 싶다하면 아래의 명령어 실행 (UIKitCore Library에 포함되어있는 무려 12만개가 넘는 Symbol Table이 출력됨)

```sh
(lldb) image dump symtab UIKitCore -s address #  -s address 옵션은 주소값으로 정렬
```

- 출력된 Symbol Table에는 해당 모듈에 포함되어있는 Variable, Method, Metadata, Protocol 등 모든 Symbol이 들어있음

![lldb_image_dump_summary](https://yagom.net/wp-content/uploads/2020/02/lldb_image_dump_summary-1024x449.png)

- Symbol의 Prefix로 `_` 가 붙어있는 이 Symbol들은 공식 문서에는 나와있지않은 Private API들.  위의 사진에서는 UIStatusBar의 priavte api 들을 캡쳐뜬건데 StatusBar의 Battery, Cellular Item에 접근 가능한걸 알 수 있음. 물론 Apple에서 제공하는 Library의 Private API 이용은 AppStore Reject 사유가 되기 때문에, 디버깅할때만 사용하는것이 좋음. 
- 3rd Party Library를 제공받아 사용 할 때에도, 공개되지 않은 Method나 Class와 같은 Symbol들을 확인할 때 유용한 Command 일 듯. 
- 언제 private api 사용해보고 싶을지 모르니 일단 사용법 링크 남겨둠 
  - [iOS Private API 사용하기, Swift](https://bomcat.tistory.com/entry/iOS-Private-API-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-Swift)
  - [Calling Hidden/Private API from Swift in Style](https://medium.com/swlh/calling-ios-and-macos-hidden-api-in-style-1a924f244ad1)

### Image Lookup

#### `image lookup` 명령어

- 앞에서 살펴본 `image dump` 명령을 통해 나온 Module 정보들은 유용하긴 하지만, 너무 많기 때문에 이 내용들을 필터링 해서 볼 수 있도록 해줌

```
(lldb) image lookup
```

- `(lldb) help image lookup` Command를 통해 알아보면 아래와 같은 다양한 필터링 Option들을 제공하고 있음을 확인 가능

```shell
  # 함수 이름 (--function)
  (lldb) image lookup -F "functionName"
  # 주소값 (--address)
  (lldb) image lookup -a "0x00address"
  # 파일 이름 (--filename)
  (lldb) image lookup -f "FileName.swift"
  # 라인 번호 (--line)
  (lldb) image lookup -f "FileName.swift" -l 15
  # 정규식 이용 (--regex)
  (lldb) image lookup -rn "regexExpression"
```

- 특정 상황에서 원하는 Symbol들을 보다 쉽게 찾아볼 수 있음.

  예를 들면, Animation을 구현하다가 CALayer에서 제공하는 setter가 어떤게 있는지 궁금해질 때, 이렇게 정규식을 이용하면 간단하게 모든 setter API를 출력 가능

```shell
  (lldb) image lookup -rn '\[CALayer\ set'
```

### Crash Log Symbolicate 해보기

- `(lldb) image lookup` Command는 **Symbolicate** 되지않은 **Crash Log**를 살펴볼 때 굉장히 유용

- 다음 이미지를 보는 차례대로 
  - 전혀 Symbolicate 되지 않은 형태의 Log
  - 부분적으로 변환된 Log
  - 우리가 알아볼 수 있는 형태로 모두 Symbolicate 된 Log

![symbolicate](https://yagomdotnet-bucket.s3.ap-northeast-2.amazonaws.com/wp-content/uploads/2020/03/15162015/29486155-c8b7731c-84f0-11e7-815b-114faebaa5d5.png)

- 만약 Application의 Crash Log를 확인 할 때, Unsymbolicated Crash Log라면 해석 방법은?

```shell
    0  The Elements             0x000000010003fc20 0x100034000 + 48160
    1  UIKit                    0x0000000187480070 0x187438000 + 295024
    2  UIKit                    0x000000018747feb0 0x187438000 + 294576
    ...
```

- 첫번째 줄의 Crash Log 는 차례대로 아래와 같은 Symbol에 정보들을 제공
  - **Binary Image Name** : The Elements 는 Crash가 발생한 Main Application 실행 파일
  - **Stack Address** : 해당 Symbol의 Stack 메모리 내 주소값 (`0x000000010003fc20`)
  - **Load Address** : Application이 Load되어있는 주소값 (`0x100034000`)
  - **Offset**: StackAddress와 LoadAddress 사이의 Offset ( == *StackAddress* – *LoadAddress* ) ( `48160`)

- 위 정보들을 이용해 Crash를 일으킨 Symbol의 Address를 찾아서, **dSYM 내의 문제아 Symbol의 Address를 찾아 어떤 놈인지 밝혀낼** 수 있음

```shell
symbol address = slide + stack address - load address
               = slide + offset
# slide value는 32bit architecture의 경우  0x4000, 64bit architecture의 경우  0x100000000
```

- 이에 따라 Symbol Address를 계산해보면,  0x10000BC20 이 문제 지점임을 파악 가능

```
# 참고: 48160 (10진수) == BC20 (16진수)
symbol address = 0x100000000 + BC20 = 0x10000BC20
```

- 마지막으로, 0x10000BC20 지점에는 어떤 Symbol이 있는지 확인해보면 되는데, 이때 LLDB를 사용!
  -  Terminal을 열고 `$ lldb`를 통해 LLDB Console을 오픈 (Xcode LLDB Console이 아닌 **Terminal**에서도 LLDB를 실행할 수 있다는데 난 왜 잘 안되지 흠 🤔)
  -  `(lldb) target create "dSYM 경로"`를 통해 lldb를 dSYM File에 Attach
  -  `(lldb) image lookup --address 0x10000bc20` Command를 통해 문제의 Symbol 위치를 검색
  -  HelloDebugger.ViewController.init 에서 Crash가 났다는 것 확인 가능! (흠,, 첨부한 사진에서는 init 에서 났다는거 알 수 있나?)

![lldb_dsym](https://yagom.net/wp-content/uploads/2020/02/lldb_dsym-1024x179.png)

## language

#### `language` 명령어

- `objc`, `swift` 등의 subcommand 를 추가 입력 후에 해당 언어에 대한 특정 기능을 수행할 수 있음

```sh
(lldb) language swift refcount self
# refcount data: (strong = 1, unowned = 0, weak = 0)
```

- 자세한 정보는 `help language`, `help language swift` 등의 명령어를 통해 살펴볼 수 있음



## 출처

- [LLDB 정복](https://yagom.net/courses/start-lldb/)

- [[WWDC 2018] Advanced Debugging with Xcode and LLDB](https://developer.apple.com/videos/play/wwdc2018/412/)

- [[WWDC 2019] LLDB: Beyond "po"](https://developer.apple.com/videos/play/wwdc2019/429/)

- [What's the difference between "p" and "po" in Xcode's LLDB debugger?](https://stackoverflow.com/questions/28806423/whats-the-difference-between-p-and-po-in-xcodes-lldb-debugger)

- [[Xcode][LLDB]Debugging With Xcode and LLDB](https://minsone.github.io/ios/mac/xcode-lldb-debugging-with-xcode-and-lldb)

- [Advanced Debugging — Part 1: LLDB Console](https://betterprogramming.pub/advanced-debugging-part-1-lldb-console-5fe8cff13a93)

- [Debugging Swift code with LLDB](https://medium.com/flawless-app-stories/debugging-swift-code-with-lldb-b30c5cf2fd49)

- [GDB to LLDB command map](https://lldb.llvm.org/use/map.html)
- [Xcode와 LLDB의 힘 : 실시간 애플리케이션 관리](https://ichi.pro/ko/post/110124929818574)
- [LLDB(Low-Level Debugger)란?](https://yagom.net/courses/start-lldb/lessons/lldblow-level-debugger%eb%9e%80/)

  

### 연관 WWDC

- [[WWDC 2019] Debugging in Xcode 11](https://developer.apple.com/videos/play/wwdc2019/412/)
- [[WWDC 2021] Discover breakpoint improvements](https://developer.apple.com/videos/play/wwdc2021/10209/)


### 그 외 참고 할만한 사이트
- [[Xcode][LLDB]Debugging With Xcode, LLDB and Chisel](http://minsone.github.io/ios/mac/xcode-lldb-debugging-with-xcode-lldb-and-chisel)
