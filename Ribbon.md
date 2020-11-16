# Ribbon

- Spring Cloud에서 사용하는 **클라이언트 사이드 로드밸런서**

## Server Side Load Balancing

- 일반적으로 사용하는 Load Balancer는 **서버사이드 로드밸런싱을 처리하는 L4 Switch**와 같은 **하드웨어** 장비



![img](https://blog.kakaocdn.net/dn/bs2ZBk/btqA7q3C7eS/BY7HL0gFSQKi4BUFXi59ck/img.png)

- 위는 일반적인 L4 switch 기반의 로드 밸런서. 
- **Client는 L4의 주소**만 알고 있으면되며 모든 로드 밸런싱은 L4에서 처리
- 하드웨어 기반의 서버 사이드 로드 밸런서의 단점은 기본적으로 **별도의 스위치 장비가 필요**하기 때문에 상대적으로 **비용**이 많이 소모되게 되며 **유연성**도 떨어짐.
- 또한 **서버 목록의 추가** 는 **수동**으로 보통 이뤄짐.
- 이러한 단점 때문에  MSA에서는 클라이이언트 사이드 로드밸런서가 주로 사용 됨.

## Client Side Load Balancing

- **MSA**에서는 하드웨어보다는 **소프트웨어적**으로 구현된 **클라이언트사이드 로드밸런싱**을 주로 이용



![img](https://blog.kakaocdn.net/dn/bXhzwf/btqA8iRN3zt/nQqdlZZZrOkVUDdGGBm1k0/img.png)



## 출처

- [[MSA] Spring Cloud Ribbon - 개념과 실습편](https://sabarada.tistory.com/54)

