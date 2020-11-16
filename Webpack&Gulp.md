# Webpack과 Gulp

- Web front-end **build tool**
- **Webpack**:  **Package Bundler** 로서 **종속성**을 가진 애플리케이션 모듈을 **정적인 소스들로 재생산**
- **Gulp**: **Task Runner** 로서  **반복 가능한 특정 작업을 자동화**

## Build Tool?

- **JavScript와 CSS를 축소**하고 **단위 테스트**도 수행하며 프로젝트에 필요한 **자산들(Image, Font)을 효율적으로 관리**
- **패키지화**까지 진행 가능 
- 프로젝트의 성능과 개발의 편의, 그리고 개발 속도가 향상

- 프론트엔드 빌드 툴로는 `Gulp`와 `Grunt` 그리고 `Webpack` 세가지가 활성화.

## Webpack

- JavaScript 애플리케이션을 위한 **Package Bundler**

- **종속성**을 가진 애플리케이션 모듈을 **정적인 소스들로 재생산**

- 애플리케이션을 처리할 때 필요한 **모든 모듈을 종속성 그래프로 반복적으로 작성**한 다음 모든 모듈을 브라우저에서 로드 할 수 있는 **하나의 Bundle로 패키지화**한다. 즉 **어떤 소스들을 하나의 패키지화** 하는 것.

- **종속성 관리** 가능

- Gulp의 전처리 작업도 지원.

- 초기 구축에 대한 시간적인 비용이 많이 투자되며 Learning Curve가 김

  ![cover](https://kdydesign.github.io/2017/07/27/webpack/cover.png)

## Gulp

-  **Task Runner** 로서  **반복 가능한 특정 작업을 자동화**.
-  번거로운 작업들이나 시간적인 소모가 많이 들어가는 작업을 **자동화**하여 쉽게 처리 가능.
-  JavaScript 테스트 실행 및 파일 병합
-  js, css, html등의 자산 파일을 압축
-  Node **stream 기반**으로 **빠른 빌드 속도**를 제공
-  작업을 정의하고 실행하는 것이 수월
-  러닝 커브 낮고, 코드 가독성 좋음.
-  Web pack과 같이 모든 모듈에 대한 종속성 관리가 이루어지지 않기에 규모가 큰 프로젝트에서 패키지화하기가 쉽지 않다

## 출처

- [Webpack 개념잡기](https://kdydesign.github.io/2017/07/27/webpack/)

