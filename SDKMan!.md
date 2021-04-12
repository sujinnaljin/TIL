# SDKMAN!

- `SDMMAN!`은 대부분의 유닉스 기반 시스템에서 여러가지 **SDK(Software Development Kits)의 병렬 버전을 관리**하기 위한 도구

- 아래 명령어로 sdkman 을 설정해주면 되는데, ec2 에서 설정할 때는 inbound, outbound 규칙에 443 port 추가해준다 (ssl)

  ```sh
  sudo curl -s "https://get.sdkman.io" | bash
  ```

  
**현재 사용중인 개발 도구의 버전 확인**

```
$ sdk current       # sdkman으로 관리하는 모든 도구 버전 확인
$ sdk current java  # java 버전 확인
```

  **설치 가능한 버전 목록 보기**

```
$ sdk list java
$ sdk list scala
```

  **개발 도구 설치**

  ```
$ sdk install java                  # latest stable 버전의 Java 설치
  $ sdk install java 11.0.4.hs-adpt   # list 확인 후 identifier를 선택할 것
$ sdk install scala 2.12.1          # Scala 2.12.1 설치
  ```

  **개발 도구 삭제**

  ```
$ sdk uninstall java 11.0.4.hs-adpt
  ```

  **현재 터미널에서 사용할 버전 지정**

  ```
$ sdk use scala 2.12.1
  ```

  **default 버전 지정**

  ```
  $ sdk default scala 2.11.6
  ```



# 출처

- [sdk 명령어 (sdkman)](https://johngrib.github.io/wiki/sdkman/)

