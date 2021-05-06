# Dyld

- Dyld는 **앱 사용 준비를 담당**하는 Apple의 프로그램

- **Macho-O 실행 파일이 dyld에 의해 로드**되면 앱이 시작 된다

- dyld의 작업 중 일부는 소스 코드의 유형을 설명하는 **바이너리 메타 데이터의 포인터를 "리베이스"** 하는 것

  앱은 시작될 때마다 **[ASLR](https://github.com/sujinnaljin/TIL/blob/master/ASLR%20(Address%20space%20layout%20randomization).md)** (주소 공간 레이아웃 무작위화)로 인해 **메모리의 다른 위치에 배치**되는데, 이때 문제는 앱 바이너리에 하드 코딩 된 주소도 임의 시작 위치로 오프셋된다는 것. **Dyld**는 **고유 시작 위치를 고려하도록 모든 포인터를 리베이스**하여 이를 수정. 

![image](https://user-images.githubusercontent.com/20410193/117299481-8f361f80-aeb3-11eb-881b-f361536e0a43.png)
가장 먼저 실행된다 홀리
# 출처

- [Why Swift Reference Types Are Bad for App Startup Time](https://medium.com/geekculture/why-swift-reference-types-are-bad-for-app-startup-time-90fbb25237fc)
