# ElasticSearch

- Apache Lucene( 아파치 루씬 ) 기반의 Java 오픈소스 **분산 검색 엔진**입니다.
- **방대한 양의 데이터**를 신속하게, 거의 **실시간( NRT, Near Real Time )으로 저장, 검색, 분석**할 수 있습니다.
- Elasticsearch는 검색을 위해 단독으로 사용되기도 하며, **ELK( Elasticsearch / Logstatsh / Kibana ) 스택**으로 사용되기도 합니다.
- **ELK 스택이란?**
  - [**Logstash**](https://github.com/sujinnaljin/TIL/blob/master/Logstash.md) : 다양한 소스( DB, csv파일 등 )의 로그 또는 트랜잭션 **데이터를 수집, 집계, 파싱**하여 Elasticsearch로 전달

  - **Elasticsearch** : Logstash로부터 받은 **데이터를 검색 및 집계**를 하여 필요한 관심 있는 **정보를 획득**

  - [**Kibana**](https://github.com/sujinnaljin/TIL/blob/master/Kibana.md) : Elasticsearch의 빠른 검색을 통해 데이터를 **시각화** 및 모니터링


![img](https://t1.daumcdn.net/cfile/tistory/993B7E495C98CAA706)

## 역색인

- Elasticsearch가 **빠른 이유**는 **inverted index( 역색인 )**에 있습니다. 

  책에서 맨 앞에 볼 수 있는 목차가 index이고, **책 맨 뒤에 키워드**마다 찾아볼 수 있도록 찾아보기가 inverted index입니다.

- Elasticsearch는 **텍스트를 파싱해서 검색어 사전**을 만든 다음에 **inverted index 방식으로 텍스트를 저장**합니다.

  ```
  "Lorem Ipsum is simply dummy text of the printing and typesetting industry"
  ```

  해당 문장을 모두 파싱해서 각 단어들( Lorem, Ipsum, is, simply .... )을 저장하고, 대문자는 소문자 처리하고, 유사어도 체크하고... 등의 작업을 통해 텍스트를 저장합니다. 때문에 RDBMS보다 **전문검색( Full Text Search )에 빠른 성능**을 보입니다.

  ( [참고 ](https://www.slideshare.net/kjmorc/ss-80803233)[링](https://www.slideshare.net/kjmorc/ss-80803233)[크](https://www.slideshare.net/kjmorc/ss-80803233) - 9페이지 )

## Elasticsearch 아키텍쳐

![img](https://t1.daumcdn.net/cfile/tistory/99A97A355C98D42D2E)

출처 : https://github.com/exo-archives/exo-es-search

- **클러스터( cluseter )** : Elasticsearch에서 **가장 큰 시스템 단위**로, 최소 하나 이상의 노드로 이루어진 **노드들의 집합**입니다.

  서로 **다른 클러스터**는 데이터의 **접근, 교환을 할 수 없는 독립적**인 시스템으로 유지되며, 여러 대의 서버가 하나의 클러스터를 구성할 수 있고, 한 서버에 여러 개의 클러스터가 존재할수도 있습니다.

- **노드( node )** : Elasticsearch를 구성하는 **하나의 단위 프로세스**를 의미합니다. 그 역할에 따라 아래와 같이 구분할 수 있습니다.

  - **master-eligible node** : **클러스터를 제어**하는 **마스터로 선택할 수 있는 노드**로, 다음과 같은 역할을 합니다.
    - 인덱스 생성, 삭제
    - 클러스더 노드들의 추적, 관리
    - 데이터 입력 시 어느 샤드에 할당할 것인지
  - **Data node** : **데이터**와 관련된 **CRUD 작업**과 관련있는 노드입니다. 이 노드는 CPU, 메모리 등 자원을 많이 소모하므로 모니터링이 필요하며, master 노드와 분리되는 것이 좋습니다.
  - **Ingest node** : 데이터를 변환하는 등 **사전 처리** 파이프라인을 실행하는 역할을 합니다.
  - **Coordination only node** : **data node와 master-eligible node의 일을 대신**하는 노드입니다. **대규모 클러스터**에서 큰 이점이 있습니다. **로드밸런서와 비슷한 역할**을 한다고 보면 됩니다.

## 인덱스( index ) / 샤드( Shard ) / 복제( Replica )

흔히 사용하고 있는 관계형 DB는 Elasticsearch에서 각각 다음과 같이 대응시킬 수 있습니다.

![img](https://t1.daumcdn.net/cfile/tistory/998444375C98CC021F)

출처: https://www.slideshare.net/deview/2d1elasticsearch

- Elasticsearch에서 **index**는 RDBMS에서 **database와 대응**하는 개념입니다.

- **샤딩( sharding )**은 **데이터를 분산해서 저장**하는 방법을 의미합니다. RDBMS에서 **Physical partition과 대응**하는 개념입니다.

  즉, Elasticsearch에서 스케일 아웃을 위해 index를 여러 shard로 쪼갠 것입니다. 기본적으로 1개가 존재하며, 검색 성능 향상을 위해 클러스터의 샤드 갯수를 조정하는 튜닝을 하기도 합니다.

- **replica**는 **또 다른 형태의 shard**라고 할 수 있습니다. **노드를 손실했을 경우** 데이터의 신뢰성을 위해 샤드들을 복제합니다. 따라서 replica는 **서로 다른 노드에 존재**할 것을 권장합니다.

  아래 사진에서 보는 바와 같이 Replica1은 Node2에 존재하는 것을 확인할 수 있습니다.

![img](https://t1.daumcdn.net/cfile/tistory/991563425C98CB341A)

출처 : https://stackoverflow.com/questions/19838825/what-are-elasticsearch-indices#answer-19839840

## Elasticsearch 특징

Elasticsearch는 다음과 같은 특징이 있습니다.

- **Scale out** : 샤드를 통해 **규모가 수평적으로 늘어**날 수 있음

- **고가용성** : Replica를 통해 **데이터의 안정성**을 보장

- **Schema Free** : Json 문서를 통해 데이터 검색을 수행하므로 스키마 개념이 없음

- **Restful** : 데이터 CURD 작업은 HTTP Restful API를 통해 수행




### MultiSearch API

한 번의 API 요청으로 대량으로 document를 **조회하는** 방법

### Bulk API

한 번의 API 요청으로 대량으로 document를 **추가하는** 방법. Bulk API를 통해 document를 update, delete도 할 수 있습니다.



## 출처

- [🙈[Elasticsearch] 기본 개념잡기🐵](https://victorydntmd.tistory.com/308)
- [🙈[Elasticsearch] 대량 추가/조회 ( Bulk API, MultiSearch API )🐵](https://victorydntmd.tistory.com/316)

- [ELK Stack Tutorial – Discover, Analyze And Visualize Your Data Efficiently](https://www.edureka.co/blog/elk-stack-tutorial/)
