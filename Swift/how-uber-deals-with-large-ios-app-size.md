# Uber가 대규모 iOS 앱 크기를 다루는 방법

## machine code outlining

기계 코드 단에서 반복되는게 많아서 중간에 별도의 컴파일 단계를 둬서 **machine code outlining** 진행. 

머신 코드 아웃라이닝이란 **반복되는 인스트럭션의 sequence 를 동일한 func 호출로 대체**하는 것

![img](https://1fykyq3mdn5r21tpna3wkdyi-wpengine.netdna-ssl.com/wp-content/uploads/2021/02/pasted-image-0-7-1024x492.png)

모듈별로 m/c outlining 을 하는게 아니라 각 **모듈**에서 뽑아져나온 LLVM IR 을 **llvm link 를 통해 하나의 LLVM IR 로 합친**뒤 **최적화 및 아웃라이닝** 진행.

![img](https://1fykyq3mdn5r21tpna3wkdyi-wpengine.netdna-ssl.com/wp-content/uploads/2021/02/pasted-image-0-6-1024x347.png)

*기본 iOS 빌드 파이프 라인.*

![img](https://1fykyq3mdn5r21tpna3wkdyi-wpengine.netdna-ssl.com/wp-content/uploads/2021/02/pasted-image-0-11-e1614289252657-1024x364.png)

 *iOS 앱에서 전체 프로그램 최적화를 활용하기위한 새로운 빌드 파이프 라인.*

## repeated machine outlining

기존 llvm 의 머신 코드 아웃라이닝은 **greedy 알고리즘** 씀. 하지만 이건 **순간 최적**이기에 전체 최적은 아님.

이런 문제를 해결하기 위해 **repeated machine outlining** 사용.  이 아이디어는 그리디 알고리즘을 사용하여 이전과 같이 수익성이 가장 높은 다음 패턴을 선택하는 것이지만, 이미 outline 된 이전 후보를 버리는 대신, **새 후보에 동일한 알고리즘을 계속 반복적으로 적용**하는 것.

![img](https://1fykyq3mdn5r21tpna3wkdyi-wpengine.netdna-ssl.com/wp-content/uploads/2021/02/pasted-image-0-12-1024x250.png)



# 출처

- [Uber가 대규모 iOS 앱 크기를 다루는 방법](https://eng.uber.com/how-uber-deals-with-large-ios-app-size/)

