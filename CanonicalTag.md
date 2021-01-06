# Canonical Tag

- **컨텐츠가 중복되거나 유사한 서로 다른 url (페이지)들을 하나의 표준 페이지로 통합** 설정해주는 태그

- 아래 url 들은 **같은 컨텐츠를 표시**하고 있기 때문에 **우리에겐 차이가 없어** 보일 수 있지만, **검색엔진의 입장**에서는 **url 이 다르**기 때문에 **전혀 다른 페이지**로 인식 될 수 있음.

  - http://example.com
  - http://example.com/
  - http://www.example.com/
  - https://example.com
  - https://example.com/
  - https://www.example.com/

- 동일한 컨텐츠를 나타내는 여러 page를 검색결과로 노출하는 것은 사용자의 검색 만족도를 떨어트리기 때문에, **다른 URL 상에 중복 콘텐츠**가 있다면 구글은 아래와 같은 **패널티 부과**

  - 웹사이트의 **랭킹 하락** 또는 **색인 삭제**
  - **하나를 표준 버전으로 선택**하여 크롤링하고, **나머지** 모든 URL은 중복 URL로 간주하고 **빈도를 줄여 크롤링** 

- 따라서 **Canonical 태그 지정**을 통해 이용해서 검색엔진 **크롤러에게 선호 URL**이 무엇인지 **명시**

- 즉, **중복**되거나 같은 **콘텐츠**를 가진 **여러 페이지**(여러 URL)가 존재할 때 **어떤 페이지가 오리지널 페이지**인지, 어떤 페이지를 크롤링 하고, 색인해야 하는지에 대한 정보를 제공하는 것

-  HTML `<head></head>` 상의 태그로, 아래와 같이 `rel="canonical"` 을 적고, `href=""` 안에 표준화할 url을 적는다

  ```html
  <link rel="canonical" href="https://example.com/"/>
  ```

## 출처

- [캐노니컬 태그 (Canonical tag), 검색엔진 최적화](https://myseolabo.com/seo/canonical-tag/)
- [4가지 사례로 알아보는 올바른 캐노니컬 태그 적용 방법](https://www.twinword.co.kr/blog/how-to-apply-canonical-tag-properly/)

