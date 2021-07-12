# NanoID

- **범용 식별자**
- NanoID로 **UUID 대체** 가능
- NanoID는 UUID보다 **큰 알파벳을 사용**하기 때문에 **더 짧은 ID** (21자) 로 더 긴 UUID (36자) 와 같은 역할을 할 수 있음 
- UUID에 비해 NanoID는 **크기가 4.5배 작음** (크기: 108바이트)
- 종속성 없음
- NanoID를 사용하는 데 큰 단점이나 제한 사항은 없지만, NanoID를 테이블의 기본 키로 사용하는 경우 클러스터형 인덱스와 동일한 column를 사용하면 문제가 발생. 이는 **NanoID가 순차적이지 않**기 때문.
  - 클러스터형 인덱스: 해당 키 값을 기반으로 테이블이나 뷰의 데이터 행을 정렬하고 저장. 데이터 행 자체는 한 가지 순서로만 저장될 수 있으므로 테이블당 클러스터형 인덱스는 하나만 존재.
- 점차 JavaScript 쪽에서 가장 인기 있는 unique ID generator가 되고 있음



# 출처

- [Why is NanoID Replacing UUID?](https://blog.bitsrc.io/why-is-nanoid-replacing-uuid-1b5100e62ed2)
- [클러스터형 및 비클러스터형 인덱스 소개](https://docs.microsoft.com/ko-kr/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described?view=sql-server-ver15)

