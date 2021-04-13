# github action과 aws code deploy를 이용한 배포 자동화

## TL;DR;

1. Github Action .yml 파일 트리거해서 EC2에 코드 배포 요청

   - 프로젝트 빌드	
   - jar 파일을 zip 으로 압축해서 S3에 업로드
   - Code Deploy에서 해당 파일을 설정된 배포그룹 (EC2) 에 배포 요청 (즉, 배포할 EC2 인스턴스 내부에 있는 CodeDeploy Agent에게 배포 명령)
2. EC2에 설치된 CodeDeploy Agent는 요청에 따라 jar 파일을 S3에서 받아서 `appspec.yml` 을 보고 프로젝트 저장 위치 결정 및 추가 동작 등 실행 (즉 실제로 배포를 수행하는 것은 Agent)

![img](https://blog.kakaocdn.net/dn/bYo3ET/btqG1YAlZ2D/wEWdrImeoGHsC1OS92OTk0/img.png)

*이미지 출처: https://wbluke.tistory.com/39*

## 1. 프로젝트 빌드 (빌드 자동화 using Github Action)

Github Action에 .yml 파일을 작성해주면 특정 조건때마다 .yml 파일이 실행되고, 해당 내용이 성공적으로 실행되었는지 여부를 알려준다.

> github action은 CI/CD, github에서 제공하는 패키지 관리 및 올인원 자동화 서비스
>
> 손쉽게 개인 Github 코드 저장소에서 빌드/배포 환경을 구축할 수 있는 도구

![image](https://user-images.githubusercontent.com/20410193/114492892-72138580-9c54-11eb-9641-5a1ce8594951.png)

아래의 코드는 `develop` 브랜치에 push 될때마다 gradlew 실행 파일을 이용해서 빌드하는 내용이다.

```yml
# 언제 액션이 이루어질 지 정할 수 있다 (트리거 조건). 아래에 브랜치를 지정해주면 된다.
# 우리는 develop 브랜치에 Push되면 자동 배포하도록 정의할 것이다.
on:
  push:
    branches:
      - develop

# 액션의 이름
name: Deploy String boot to Amazon EC2
env:
  PROJECT_NAME: action_codedeploy

# 아래의 Job들이 깃헙 액션에서 진행된다.
jobs:
   # 하나의 job을 정의
  deploy:
    # job의 이름
    name: DEPLOY
    # 빌드가 어느 운영체제에서 돌아가는지
    runs-on: ubuntu-18.04

    # step은 job의 하위 집합. step에서 정의한 작업을 순차적으로 진행한다.
    steps:
      # 위에서 정의한 브랜치로 체크아웃
      - name: Checkout
        uses: actions/checkout@v2

      # 자바 버전 설정
      - name: Set up JDK 11
        uses: actions/setup-java@v1
        with:
          java-version: 11
          
      # GradleWrapper에 실행 권한을 부여. java나 gradle을 설치하지 않고 바로 빌드할 수 있게 해주는 역할. 
      # gradle은 안드로이드의 기본 빌드 시스템
      - name: Grant execute permission for gradlew
        run: chmod +x gradlew
        shell: bash

      # Gradle Wrapper를 활용해 빌드
      - name: Build with Gradle
        run: ./gradlew --debug build #:test 에러 방지 위해 --debug 옵션 추가
        shell: bash

```

## 2. (AWS) 배포용 S3 버킷 생성

CodeDeploy를 이용해 배포를 하려면 S3를 통하게된다. 

프로젝트를 zip으로 압축 후 S3에 업로드하고, CodeDeploy가 해당 파일 경로를 이용해 배포 대상에 배포를 요청한다.

그렇기 때문에 배포용 S3가 필요하다.

- S3 -> 버킷 -> 버킷 만들기 -> **이름 설정** -> **퍼블릭 액세스 차단 : 모든 퍼블릭 엑세스 차단** (IAM 이라는 또 다른 AWS 의 권한 서비스를 이용하여 최소한의 접근 권한만 가지고 플로우를 구축할 것이기 때문)

## 3. (AWS) CLI용 IAM user 생성

이제 Github Actions 에서  AWS CLI 명령을 통해 S3 버킷에 접근할 수 있도록 권한을 생성해야 한다. 나중에 Github Actions 에서 CodeDeploy 접근도 할 것이기 때문에 이 권한도 넣어서 같이 IAM 을 생성한다.

> IAM 은 AWS 서비스에 대한 엑세스 권한을 최소한으로 부여하고 관리할 수 있게 해주는 서비스
>
> 외부 서비스에서 AWS 서비스에 접근하려고 할 때도 물론이고, AWS 서비스 간에 접근하고자 할 때도 권한 부여 가능

- IAM -> 액세스 관리 -> 사용자 -> 사용자 추가 -> 사용자 이름 설정 -> **액세스 유형 : 프로그래밍 방식 액세스** -> 권한 설정 : 기존 정책 직접 연결 -> **정책 필터 : S3FullAccess , CodeDeployFullAccess** -> 사용자 만들기 -> **accees-key 및 secret-access-key 저장**

액세스 유형 : 프로그래밍 방식 액세스 을 선택한 이유는 AWS CLI 명령을 통해 엑세스할 것이기 때문이다. 해당 옵션으로 선택하면 엑세스 키 ID, 비밀 엑세스 키를 발급해주는데 이는 일종의 아이디/비밀번호 라고 생각하면 된다.

## 4. Github Action .yml 파일에 .zip 압축, AWS 인증, S3 업로드 step 추가

아래와 같은 코드를 위에서 작성한 .yml 파일의 steps 하위에 추가한다

- .zip 압축

  CodeDeploy 배포를 하기 위해 S3를 통해야하는데, S3에는 압축된 파일로 올라가야한다.

  ```yml
        # 버전마다 이름을 다르게 하기 위해서 GITHUB_SHA(github action에서 제공하는 기본 환경변수) 라고 하는 해시이름을 활용하여 zip 파일을 만든다.
        # CodeDeploy를 사용하기 위해서는 S3를 거쳐야하므로, 우선 압축된 파일로 만들어주고 추후 이를 옮겨야 한다.
        - name: Make zip file
          run: zip -qq -r ./$GITHUB_SHA.zip .
          shell: bash
  ```

- AWS 인증

  오픈되면 안되는 정보들은 repository Setting->Secrets 에서 key, value로 저장 후 .yml 파일에서 $ 로 접근할 수 있다.

  우리가 Secrets에 저장할 것들은 CLI 용 IAM user을 생성할 때 나온 AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_REGION 이다.

  ```yml
        # AWS 서비스를 사용(s3 업로드)하기 위한 인증 과정 (aws cli credentials)
        # 오픈되면 안되는 정보들은 repository Setting->Secrets 에서 key, value로 저장 후 deploy 할 때 $로 사용가능
        - name: Configure AWS credentials
          uses: aws-actions/configure-aws-credentials@v1
          with:
            aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
            aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
            aws-region: ${{ secrets.AWS_REGION }}
  ```

- S3에 zip 파일 업로드

  이때 `s3://` 뒤에 이름을 aws 에서 설정한 S3 이름으로 잘 바꿔주자. 나의 경우 s3의 이름이 `manham-digin` 이다 

  ```yml
        # 배포 zip 파일을 S3에 업로드
        - name: Upload to S3
          run: aws s3 cp --region ap-northeast-2 ./$GITHUB_SHA.zip s3://manham-digin/$PROJECT_NAME/$GITHUB_SHA.zip
  ```

## 5. EC2 세팅 - jdk 및 codedeploy agent 설정

이제 우리는 S3 업로드 한 파일을 다운 받아 EC2에 배포를 진행해야하는데 이를 위해 AWS의 Code Deploy를 이용한다. EC2에서 Code Deploy를 이용하기 위해서는 Code Deploy Agent를 설치해줘야한다.

**CodeDeploy Agent** 는 EC2 인스턴스에 설치되어 **CodeDeploy의 명령을 기다리고 있는 프로그램**으로, **실제로 배포를 수행하는 것은 Agent이기 때문에 반드시 EC2 인스턴스에 설치**되어 있어야 한다.

더 자세하게는 **CodeDeploy**가 배포할 EC2 인스턴스 내부에 있는 **CodeDeploy Agent에게 배포 명령**을 내리면 **Agent**는 jar 파일을 **S3에서 다운로드** 받아서 주어진 **스크립트에 따라 배포**를 진행하는 역할을 한다.

- codedeploy를 사용하기 위해 `codedeploy agent` 설치

  ```sh
  # install aws code deploy dependency
  
  # 패키지 매니저 업데이트, ruby 설치
  sudo apt-get update
  sudo apt-get install ruby
  sudo apt-get install wget
  
  # 서울 리전에 있는 CodeDeploy 리소스 키트 파일 다운로드
  cd /home/ubuntu
  wget https://aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/latest/install
  
  # 설치 파일에 실행 권한 부여
  chmod +x ./install
  
  # 설치 진행
  sudo ./install auto > /tmp/logfile
  
  # codedeploy agent가 실행 중인지 상태 확인
  sudo service codedeploy-agent status 
  ```

- jdk 설치 

  `sudo apt install openjdk-8-jdk` or [SDKMAN!](https://github.com/sujinnaljin/TIL/blob/master/SDKMan!.md) 등 이용 가능

## 6. (AWS) EC2 의 CodeDeploy 사용을 위한 IAM 설정

**EC2**가 스스로 Codedeploy 서비스를 사용할 수 있도록 **IAM 롤을 생성 후 부여**한다

- AWS 페이지에서 IAM을 만든다

  서비스 -> IAM -> 역할 -> 역할 만들기 -> **사용 사례 : EC2** ->  **정책 필터 : CodeDeployFullAccess, S3FullAccess**

- EC2에 가서 만든 IAM 롤을 연결한다

  작업 -> 보안 -> IAM 역할 수정

## 7. appspec.yml 생성

앞서 말했듯, **CodeDeploy**가 배포할 EC2 인스턴스 내부에 있는 **CodeDeploy Agent에게 배포 명령**을 내리면 Agent는 jar 파일을 S3에서 받아서 **주어진 스크립트에 따라 배포를 진행**하는 역할을 한다.

이때 주어진 스크립트가 **AppSpec.yml** 이다. 해당 파일을 읽어 **작성된 절차대로 배포를 진행**하므로 **프로젝트 root 경로에 `appspec.yml` 파일을 생성**하고 받아온 프로젝트를 어디에 저장할지, 무엇을 실행할지 등을 정한다.

아래는 `/deploy` 폴더 경로에 프로젝트를 저장할 것이고, 설치 후 ` deploy.sh` 파일을 실행하는 코드이다.

```yml
# 배포가 실행되면 ec2에 설치된 CodeDeploy agent에서 해당 파일을보고, 받아온 프로젝트를 어디에 저장할지, 무엇을 실행할지를 정함.

version: 0.0
os: linux

files:
  - source: /
    destination: /deploy
permissions:
  - object: /deploy
    owner: ubuntu
    group: ubuntu
    mode: 755
hooks:
  AfterInstall:
    # location은 프로젝트의 root경로를 기준
    - location: deploy.sh
      timeout: 60
      runas: root
```

## 8. deploy.sh 생성

위의 `appspec.yml` 에서 실행한다고 정의한 `deploy.sh` 을 생성한다. 

배포 스크립트로, 해당 파일에서 jar를 실행해준다고 생각하면된다.

```sh
#!/usr/bin/env bash

REPOSITORY=/deploy
cd $REPOSITORY

APP_NAME=action_codedeploy
JAR_NAME=$(ls $REPOSITORY/build/libs/ | grep '.jar' | tail -n 1)
JAR_PATH=$REPOSITORY/build/libs/$JAR_NAME

CURRENT_PID=$(pgrep -f $APP_NAME)

if [ -z $CURRENT_PID ]
then
  echo "> 종료할것 없음."
else
  echo "> kill -9 $CURRENT_PID"
  kill -15 $CURRENT_PID
  sleep 5
fi

echo "> $JAR_PATH 배포"
nohup java -jar $JAR_PATH > /dev/null 2> /dev/null < /dev/null &
```

## 9. (AWS) CodeDeploy와 배포 그룹 생성 및 IAM 설정

이제 우리는 EC2의 **Code Deploy Agent에 명령을 내려 배포를 도와줄 Code Deploy를 생성**해야한다. 

**CodeDeploy** 는 애플리케이션 배포를 자동화하는 AWS 의 **배포 서비스**로, 배포 과정은 다음과 같다.

- CodeDeploy에 프로젝트의 특정 버전을 배포해 달라고 요청하면, **CodeDeploy는 배포를 진행할 EC2 인스턴스에 설치돼 있는 CodeDeploy Agent들과 통신하며 Agent들에게 요청받은 버전을 배포해 달라고 요청**
- 요청 받은 **Agent들은 코드 저장소에서 프로젝트 전체를 서버에 내려받**고, 개발 프로젝트 최상단 경로에 있는 **AppSpec.yml 파일을 읽어 해당 파일에 적힌 절차대로 배포**를 진행
- Agent는 배포를 진행한 후 CodeDeploy에게 성공/실패 등의 결과 알려줌

따라서 CodeDeploy 애플리케이션을 만들어야하는데, 그전에 얘도 **CodeDeploy를 위한 IAM Role이 필요**하기 때문에 **IAM을 생성**한다

- 서비스 -> IAM -> 역할 -> 역할 만들기 -> **사용 사례 : CodeDeploy** 

그리고 **CodeDeploy 생성** 후 연관된 **배포 그룹을 생성**할때 위에서 만든 **IAM 을 연결**해준다.

- CodeDeploy -> 배포 -> 애플리케이션 -> 애플리케이션 생성 -> **이름 설정** -> **컴퓨팅 플랫폼 : EC2/온프레미스**

- 생성 된 CodeDeploy의 **배포 그룹 생성**

  배포 그룹 생성 -> **이름 설정** -> **서비스 역할 : 위에서 만든 IAM 설정** -> 배포 유형 : 현재 위치 -> **환경 구성 : Amazon EC2 인스턴스** -> **태그 그룹 : 키 - NAME, 값 - 배포할 EC2 인스턴스 이름** -> 배포 구성 : CodeDeployDefault.AllAtOnce -> 로드 밸런서: 체크 해제

  즉 환경구성의 태그 섹션에서 **어느 곳에 배포할 것인지를 선택**하는 것인데 미리 만들어놓은 **EC2의 Name태그를 이용해 대상 설정**

## 10. Github Action .yml 파일에 코드 배포 step 추가

CodeDeploy 설정을 완료했으니 Github Actions에 CodeDeploy Step을 추가한다. 이때 바꿔야할 변수들이 몇가지 있다.

- `--application-name` 은 **Code Deploy 애플리케이션** 생성에서 설정된 이름. 나의 경우 `digin`
- `--deployment-config-name` 은 배포 그룹 설정에서 선택했던 **배포 방식**. 나의 경우 `CodeDeployDefault.OneAtATime`
- `--deployment-group-name` 은 **Code Deploy 배포 그룹** 생성에서 설정된 이름. 나의 경우 `develop`
- `--s3-location bucket` 은  **S3 버킷** 이름, 파일 타입, 파일 경로. 나의 경우 버킷이름이 `manham-digin`  

```yml
      # 코드 배포 요청
      # manham-digin s3 에 있는 파일을 digin Code Deploy 어플리케이션이 develop 배포그룹(ec2)에 배포
      - name: Code Deploy
        run: aws deploy create-deployment --application-name digin --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name develop --s3-location bucket=manham-digin,bundleType=zip,key=$PROJECT_NAME/$GITHUB_SHA.zip
```

### 전체 Github Action .yml 코드

```yml
# 언제 액션이 이루어질 지 정할 수 있다 (트리거 조건). 아래에 브랜치를 지정해주면 된다.
# 우리는 develop 브랜치에 Push되면 자동 배포하도록 정의할 것이다.
on:
  push:
    branches:
      - develop

# 액션의 이름
name: Deploy String boot to Amazon EC2
env:
  PROJECT_NAME: action_codedeploy

# 아래의 Job들이 깃헙 액션에서 진행된다.
jobs:
   # 하나의 job을 정의
  deploy:
    # job의 이름
    name: DEPLOY
    # 빌드가 어느 운영체제에서 돌아가는지
    runs-on: ubuntu-18.04

    # step은 job의 하위 집합. step에서 정의한 작업을 순차적으로 진행한다.
    steps:
      # 위에서 정의한 브랜치로 체크아웃
      - name: Checkout
        uses: actions/checkout@v2

      # 자바 버전 설정
      - name: Set up JDK 11
        uses: actions/setup-java@v1
        with:
          java-version: 11
          
      # GradleWrapper에 실행 권한을 부여. java나 gradle을 설치하지 않고 바로 빌드할 수 있게 해주는 역할
      - name: Grant execute permission for gradlew
        run: chmod +x gradlew
        shell: bash

      # Gradle Wrapper를 활용해 빌드. build.gradle
      - name: Build with Gradle
        run: ./gradlew --debug build
        shell: bash
        
      # 버전마다 이름을 다르게 하기 위해서 GITHUB_SHA(github action에서 제공하는 기본 환경변수) 라고 하는 해시이름을 활용하여 zip 파일을 만든다.
      # CodeDeploy를 사용하기 위해서는 S3를 거쳐야하므로, 우선 압축된 파일로 만들어주고 추후 이를 옮겨야 한다.
      - name: Make zip file
        run: zip -qq -r ./$GITHUB_SHA.zip .
        shell: bash
      
      # AWS 서비스를 사용(s3 업로드)하기 위한 인증 과정 (aws cli credentials)
      # 오픈되면 안되는 정보들은 repository Setting->Secrets 에서 key, value로 저장 후 deploy 할 때 $로 사용가능
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}
      
      # 배포 zip 파일을 S3에 업로드
      - name: Upload to S3
        run: aws s3 cp --region ap-northeast-2 ./$GITHUB_SHA.zip s3://manham-digin/$PROJECT_NAME/$GITHUB_SHA.zip
      
      # 코드 배포 요청
      # manham-digin s3 에 있는 파일을 digin Code Deploy 어플리케이션이 develop 배포그룹(ec2)에 배포
      - name: Code Deploy
        run: aws deploy create-deployment --application-name digin --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name develop --s3-location bucket=manham-digin,bundleType=zip,key=$PROJECT_NAME/$GITHUB_SHA.zip

```

## 11.Code Agent 재시작

이제 CodeDeploy > 배포에 들어가서 내역을 확인하면 대충 된다고 해야할텐데.. 실패가 뜰 것이다.

![image](https://user-images.githubusercontent.com/20410193/114492929-85beec00-9c54-11eb-9291-b6820b330c1e.png)

심지어 가장 초기 이벤트인 ApplicationStop 에서 죽는다?

![image](https://user-images.githubusercontent.com/20410193/114492965-95d6cb80-9c54-11eb-8895-d3305adb62bf.png)


실패 원인은 CodeDeploy Agent를 이미 시작한 후에 IAM Role을 EC2에 부여했기 때문에, Agent가 IAM Role을 가지고 있지 않기 때문이다.

따라서 EC2에 접속해서 codedeploy-agent 재실행한다.

```sh
sudo service codedeploy-agent restart
```





# 출처

- [github action과 aws code deploy를 이용하여 spring boot 배포하기(1)](https://isntyet.github.io/deploy/github-action%EA%B3%BC-aws-code-deploy%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-spring-boot-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0(1)/)
- [How to deploy using AWS CodeDeploy with GitHub Actions](hㅓ)
- [gradlew Execution failed for task ':test' 오류](https://sang12.co.kr/148)
- [github action과 aws code deploy를 이용하여 spring boot 배포하기(2)](https://isntyet.github.io/deploy/github-action%EA%B3%BC-aws-code-deploy%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-spring-boot-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0(2)/)
- [github action과 aws code deploy를 이용하여 spring boot 배포하기(3)](https://isntyet.github.io/deploy/github-action%EA%B3%BC-aws-code-deploy%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-spring-boot-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0(3)/)
- [github action과 aws code deploy를 이용하여 spring boot 배포하기(4)](https://isntyet.github.io/deploy/github-action%EA%B3%BC-aws-code-deploy%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-spring-boot-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0(4)/)
- [Github Action, AWS CodeDeploy를 사용하여 자동 배포하기!](https://bohyeon-n.github.io/deploy/devops/github_action.html)
- [Github Actions + CodeDeploy + Nginx 로 무중단 배포하기 (1)](https://wbluke.tistory.com/39)
- [Github Actions + CodeDeploy + Nginx 로 무중단 배포하기 (2)](https://wbluke.tistory.com/40?category=418851)

