# RDBMS vs. Hadoop

## TL; DR;

- **RDBMS**는 **정형 데이터**를 저장하는 반면 **Hadoop**은 **정형, 반 정형 및 비정형 데이터**를 저장

## RDBMS

- RDBMS는 관계형 모델을 기반으로하는 **관계형 데이터베이스 관리 시스템** 
- RDBMS에서 **테이블**은 **데이터를 저장**하는 데 사용되며 **키와 인덱스**는 **테이블을 연결**하는 데 도움
- **테이블**은 **데이터 요소의 모음**이며 엔티티. **행**은 테이블의 **단일 항목**을 나타내고, **열**은 **속성**을 나타냄

- 데이터 무결성, 정규화 등을 제공
- 일반적인 RDBMS로 MySQL, MSSQL 및 Oracle 많이 사용

- 쿼리를 위해 SQL을 사용

## Hadoop

- Hadoop은 Java로 작성된 Apache 오픈 소스 프레임워크
- 간단한 프로그래밍 모델을 사용하여 컴퓨터 클러스터에서 **대량의 데이터를 저장하고 처리**하는 데 도움
- Hadoop의 주요 목표는 **빅 데이터를 저장하고 처리**하는 것

- Hadoop 아키텍처에는 4 개의 모듈 존재

  1. **Hadoop common**

     Java 라이브러리 및 유틸리티가 포함. Hadoop을 시작하는 파일도 있음.

  2. **YARN** 

     작업 예약 및 클러스터 리소스 관리를 수행

  3. **HDFS** (Hadoop Distributed File System) 

     **파일을 저장**하기 위한 Layer (Hadoop 스토리지 시스템).

     마스터-슬레이브 아키텍처를 사용. 

     - Master Node: DFS(Distrbuted File System, 분산 파일 시스템)에 대한 정보들을 지니고 있으며, 자원 할당을 조절. Master Node는 2가지 Daemon을 통해 이를 처리

       - Name Node: DFS를 관리하고, 어떤 Data Block이 클러스터에 저장되어 있는지 알려줌.
       - Resource Manager: 스케줄링 및 Slave Node의 처리를 실행.

       즉, 파일 시스템 메타 데이터를 관리하고, 작업을 해야 하는 파일을 Block으로 나누어 Data Node에 전달

     - Slave Node : 실제 데이터를 가지고 있으며, job을 수행하는데 Data Node와 Node Manager를 통해 이를 처리

       - Data Node: NameNode에 물리적으로 저장된 실제 데이터를 관리
       - NodeManager: 노드의 Task를 실행

       즉, 실제 데이터를 저장하고 전달받은 파일의 읽기/쓰기 등을 수행

  4. **MapReduce Layer** 

     MapReduce를 수행하기 위한 Layer으로, **분산 계산**을 수행. 데이터를 처리하는 알고리즘이 있음. 

     마스터-슬레이브 아키텍처를 사용. 

     - Master Node : Job Tracker가 있어서 사용자로부터 Job을 요청 받고 Task Tracker에 작업 할당

     - Slave Node : Task Tracker에서는 Job Tracker로부터 할당 받은 작업을 Map-Reduce하여 결과 반환

     ![image](https://user-images.githubusercontent.com/20410193/147643470-1636fc6d-1f5e-4bb5-962f-bdaf4eb3bec0.png)

## 비교

|               | RDBMS                                                        | Hadoop                                                       |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 목적          | 관계형 모델을 기반으로하는 데이터베이스를 만들고 관리하기위한 시스템 소프트웨어 | 많은 컴퓨터를 연결하여 많은 양의 데이터 및 계산과 관련된 문제를 해결하는 오픈 소스 소프트웨어 모음 |
| 데이터 다양성 | 구조화 된 데이터를 저장                                      | 정형, 반 정형 및 비정형 데이터를 저장                        |
| 데이터 저장량 | 평균 데이터 양을 저장                                        | RDBMS보다 많은 양의 데이터를 저장                            |
| 속도          | 읽기가 빠름                                                  | 읽기 및 쓰기가 빠름                                          |
| 확장성        | 수직 확장성이 있음                                           | 수평 확장성이 있음                                           |
| 하드웨어      | 고급 서버를 사용                                             | 상용 하드웨어를 사용                                         |

# 출처

- [RDBMS와 Hadoop의 차이점](https://ko.strephonsays.com/rdbms-and-hadoop-8502)
- [[Hadoop] Hadoop의 구조와 MapReduce](https://mangkyu.tistory.com/129)

