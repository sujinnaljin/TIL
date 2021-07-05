# \<a\> 태그의 target 속성

- 링크된 문서를 클릭했을 때 문서가 열릴 위치를 명시

```html
<a target="_blank|_self|_parent|_top|프레임 이름">
```

#### 속성 값

|   속성값    |                             설명                             |
| :---------: | :----------------------------------------------------------: |
|   _blank    |      링크된 문서를 새로운 윈도우나 탭(tab)에서 오픈함.       |
|    _self    | 링크된 문서를 링크가 위치한 현재 프레임에서 오픈함.기본값으로 생략 가능. |
|   _parent   |     링크된 문서를 현재 프레임의 부모 프레임에서 오픈함.      |
|    _top     |          링크된 문서를 현재 윈도우 전체에서 오픈함.          |
| 프레임 이름 |           링크된 문서를 명시된 프레임에서 오픈함.            |

- HTML5에서는 프레임(frame)과 프레임셋(frameset)을 더 이상 지원하지 않기 때문에 _parent, _top, 프레임 이름과 같은 속성값들은 iframe 요소와 함께 주로 사용 됨

- 차이 영상을 보려면 [유투브 영상](https://www.youtube.com/watch?v=kSSn2a9V0Io) 확인

  



# 출처

- [\<a\> 태그의 target 속성](http://tcpschool.com/html-tag-attrs/a-target)
  
- [The difference between target: _top, _parent and _framename](https://www.youtube.com/watch?v=kSSn2a9V0Io)

