# Certificate와 .p12

- 애플이 **개발자를 신뢰**할 수 있는 보증서
- 애플의 하드웨어에서 특정 **소프트웨어가 동작**하는데, **애플의 허가**가 필요
- 이 **허가**는 개발자가 **certificates를 생성하고 실행하여 xcode에 설정**하면, 애플의 신뢰 대상이 되어 개발한 소프트웨어를 **실행** 할 수 있음
- 인증서를 통해 Apple이 **앱의 개발자 또는 게시자**로서 **나를 고유하게 식별하는 데 사용**
- 인증서에는 **공개 / 개인 키 쌍**이 포함 됨
- 배포 인증서의(distribution certificate) **개인 키는 애플리케이션에 서명** (signing) 하는 데 사용

## certificate 생성 방법

### 1. Mac에서 CSR(`.certSigningRequest`)  파일 생성

<img width="144" alt="스크린샷 2022-02-25 오후 2 31 29" src="https://user-images.githubusercontent.com/20410193/155659669-d012795a-9023-44f6-9d0f-3056011cc4dd.png">

이때 Key항목에 Public Key와 Private Key 도 같이 생성됨

<img width="780" alt="image" src="https://user-images.githubusercontent.com/20410193/155659691-9a781419-28b1-4351-b987-332b79b1fc07.png">

### 2. 애플 개발자 홈페이지에서 Certificates(`.cer`) 파일을 생성 및 다운로드

- 위에서 만든 **CSR파일**을 가지고, **애플에서** certificate(앱 개발시 필요, 앱 배포시 필요)를 만드는데 사용
- 마지막에 스텝에서 **다운로드** 클릭해서 컴퓨터에 받음

<img width="832" alt="image" src="https://user-images.githubusercontent.com/20410193/155659726-5d00f9d5-01aa-4338-8894-a7a786a6d255.png">

### 3. `.cer` 설치

- 다운로드한 `.cer` 파일을 찾아 더블 클릭하면 키체인에 등록됨

  <img width="756" alt="image" src="https://user-images.githubusercontent.com/20410193/155659978-48786a1f-db73-4346-9401-cfc5e83566b4.png">


## 문제점과 p12 의 등장

- 개발자 등록해서 받은 인증서 키는 한대의 맥에서만 사용할수 있음. 즉, **동일한 인증서**를 **다른 맥**에서 다운로드 받아서 설치하는 것은 **소용이 없다**는 것. 
- 이 때는 기존 개발자 인증서가 설치된 맥에서 **인증서 보내기**를 통해 **`.p12`** (인증서 교환 포맷) 형태로 산출된 파일을 **다른 맥에서 설치**하면 해결됨

### p12 생성 방법

- 인증서 관리는 키체인에서 하므로, 키체인 접근해서 **등록했던 인증서**와 **키**가 필요
- 이렇게 **인증서**, **키** 두 개를 **동시에** 클릭한 상태에서 **마우스 우 클릭**을 하여 이렇게 **2개 항목 내보내기** 클릭
- 내보내기 할때 비밀번호는 기억해뒀다가, 동료들한테 p12 파일 전달주면서 같이 말해주기

<img width="783" alt="image" src="https://user-images.githubusercontent.com/20410193/155659779-fe5dbfcd-1b0d-4cb2-9c55-d9958b875d96.png">

<img width="775" alt="image" src="https://user-images.githubusercontent.com/20410193/155659796-bec0f677-38ac-448b-9402-9b498a234072.png">

<img width="773" alt="image" src="https://user-images.githubusercontent.com/20410193/155659783-5b8e5e15-1c63-4058-9128-206720a142ae.png">

### p12 설치 확인

- Xcode > Preferences > Team 선택 > Manage Certificates 에서 존재하는 인증서 확인하면 됨

- 참고로 엑스코드 한번 껐다 켜야 제대로 반영이 됨

  ![image](https://user-images.githubusercontent.com/20410193/155659927-153effcc-e4f5-423e-b842-42a77372d539.png)
  

# 출처

- [How to create P12 certificate for iOS distribution](https://stackoverflow.com/questions/9418661/how-to-create-p12-certificate-for-ios-distribution)
- [같은 개발자가 여러 맥에서 작업할 때 인증서 문제](https://soooprmx.com/%EA%B0%99%EC%9D%80-%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80-%EC%97%AC%EB%9F%AC-%EB%A7%A5%EC%97%90%EC%84%9C-%EC%9E%91%EC%97%85%ED%95%A0-%EB%95%8C-%EC%9D%B8%EC%A6%9D%EC%84%9C-%EB%AC%B8%EC%A0%9C/)
- [여러대 맥에서 iOS개발시 인증서 복사](https://smilejsu.tistory.com/336)
- [iOS) APNs :: 인증서 발급받는 방법 (p.12, pem)](https://babbab2.tistory.com/57)
- [[ iOS \] APNS 인증서 생성 및 발급 PEM, P12 파일 만들기 1](https://tttap.tistory.com/212)


