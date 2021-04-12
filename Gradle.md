# Gradle

- 구글이 안드로이드의 기본 빌드 시스템으로 Gradle을 선택

- `gradle init` 명령어 통해 아래의 폴더 / 파일 생성 (스프링 부트 프로젝트 생성하면 기본적으로 생성된다고 함)

  ```
  project/
      gradlew
      gradlew.bat //원도우용 실행 배치 스크립트
      gradle/wrapper/
          gradle-wrapper.jar
          gradle-wrapper.properties
      build.gradle //의존성이나 플러그인 설정 등을 위한 스크립트 파일
      settings.gradle //프로젝트의 구성 정보를 기록. 하위프로젝트들이 어떤 관계로 구성되어 있는지를 기술
  ```

- `gradle build` 로 빌드가 가능하지만, 이 경우 Java나 Gradle이 설치되어 있어야 하고, 새로받은 프로젝트의 Gradle 버전과 로컬에 설치된 Gradle 버전이 호환되지 않으면 문제가 발생할 수 있다. 따라서 Wrapper를 사용 -> `./gradlew build`

# 출처

- [운영 자동화#1 — 빌드 자동화 by Gradle](https://medium.com/@goinhacker/%EC%9A%B4%EC%98%81-%EC%9E%90%EB%8F%99%ED%99%94-1-%EB%B9%8C%EB%93%9C-%EC%9E%90%EB%8F%99%ED%99%94-by-gradle-7630c0993d09) 

