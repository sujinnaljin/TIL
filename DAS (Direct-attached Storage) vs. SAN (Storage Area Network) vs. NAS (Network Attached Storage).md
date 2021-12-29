# DAS (Direct-attached Storage) vs. SAN (Storage Area Network) vs. NAS (Network Attached Storage)

## DAS (Direct-attached Storage)

- 초창기 서버에는 우리가 아는 RAM과 하드디스크, 즉 주 기억장치(primary storage)와 보조 기억장치(secondary storage)가 구분되어 있지 않았음.

  > **데이터 스토리**지란 말 그대로 **데이터(정보)를 저장**하는 공간. AWS나 구글 데이터센터에 무수히 꽂혀 있는 하드디스크에서 지금은 거의 사용되지 않는 플로피 디스크까지 모두 아울러 데이터 스토리지라고 할 수 있음

- 하지만 늘어나는 용량에 맞추다 보니 공간적인 한계에 봉착했고, 그 결과 **구분된 스토리지 시스템을 가진 보조 기억장치가 탄생**했는데, 이게 바로 **직접 연결 스토리지(Direct-attached Storage, DAS)** 개념의 첫 사례

- 이와 같이 서버와 스토리지를 구분함으로써 업무 효율을 위해 보다 정교하고 복잡한 아키텍처를 구성할 수 있게 됨

## SAN (Storage Area Network)

- 예전에는 서버의 스토리지 용량을 추가해야 할 경우, 새 디스크를 추가하거나 여유 공간이 있는 다른 서버에서 디스크를 물리적으로 빼서 추가해야 했음. 이러한 불편함을 해소하기 위해 SAN이 고안 됨
- SAN 방식은 **여러 스토리지를 하나의 네트워크에 연결**시키고, **이 네트워크에 서버를 연결해 스토리지에 접속**한다는 개념
- 네트워크에 묶인 스토리지들은 가상으로 중앙화된 논리 볼륨(logical volume)을 형성하고, 필요에 따라 각 서버에 공간을 논리적으로 할당 가능. 사용자는 LUN(Logical Unit Number)이라는 고유 번호를 통해 가상으로 할당된 디스크 드라이브에 연결 됨.
- **네트워크의 각 컴퓨터**는 컴퓨터에 직접 연결된 로컬 디스크처럼 **SAN 스토리지에 액세스** 가능
- SAN 환경을 구성하기 위해서는 스토리지와 서버를 중계하는 역할을 하는 **SAN 스위치** 필요. 각 서버와 스토리지를 **광 케이블**로 SAN 스위치와 연결해 데이터를 주고 받음. 
- **광케이블**을 사용하기 때문에 데이터 접근이 **빠르고** LAN을 사용하지 않아 **네트워크 부하를 최소화**할 수 있음.
- SAN을 사용하는 경우는 아래와 같음
  - 데이터베이스: 온라인 금융 거래와 같이 **빠른 속도**를 요구하고 **지연에 민감**하며 대규모 데이터베이스를 다루는 환경에 적합
  - 가상화 환경: 가상 머신과 호스트간 **빠른 입출력 속도**를 제공해야 하는 대규모 가상화 구축 환경에 적합
  - 영상 편집: 후반 작업과 같은 영상 편집 작업에 있어서 **빠른 전송 속도와 낮은 지연**은 필수 불가결. 이 때문에 워크스테이션을 DAS로 직접 연결해 사용하는 경우도 있지만 효율성을 위해 고성능 SAN을 사용
- SAN의 주요 스토리지 프로토콜은 다음과 같음
  - iSCSI (Internet Small Computer Systems Interface)
  - 파이버 채널 (Fibre Channel)
  - iSER (iSCSI Extensions for RDMA)
  ![image](https://user-images.githubusercontent.com/20410193/147643350-69922cb2-c529-4a45-be38-51cc13f57921.png)
## NAS (Network Attached Storage)

- 스토리지에 접속하는 사용자가 증가하고 공유가 필요한 자료가 많아지면서 보다 쉽고 편리하게 데이터를 공유할 방법이 필요했음. 이러한 배경 속에서 **SAN과 함께 등장**한 것이 바로 **네트워크 결합 스토리지**(**Network-attached Storage,  NAS**).
- **NAS** 도 SAN과 마찬가지로 **네트워크로 연결된 공유 스토리지 솔루션**
- SAN은 여러 기기로 이루어진 로컬 네트워크인 반면, NAS는 **LAN**(Local Area Network)에 연결. **스토리지**를 SAN 스위치와 연결하지 않고 **이더넷 케이블을 사용해 네트워크에 연결**하는 것. 단적인 예로, 인터넷 공유기에 NAS를 연결하면 같은 이더넷 네트워크에 연결된 PC로 NAS에 접근할 수 있음. 
- 범용적인 네트워크를 사용하는 관계로 **설치나 유지 관리**가 쉽지만, 같은 이더넷에 연결된 장비들과 네트워크 자원을 공유하기 때문에 **대역폭에 한계**가 있을 수 있음

- NAS를 사용하는 경우는 아래와 같음

  - 파일 공유: NAS의 주 용도이며, 데이터를 중앙화하고 스토리지 공간을 효율적으로 활용하는데 적합. 개인용부터 중소기업, 대기업 사무실까지 광범위하게 활용 됨.

  - 가상화 환경: 고성능 NAS의 경우 소규모 가상화 환경을 운영하거나 가상 데스크톱 환경을 구축하는데 적합한 성능과 기능을 가짐.

  - 아카이브: 스토리지 공간을 필요에 따라 확장할 수 있는 스케일 아웃 NAS의 경우 단순히 데이터를 묵혀 두는 방식의 아카이브가 아닌, 필요에 따라 종종 접근이 가능한 아카이브로서 적합

- NAS의 주요 스토리지 프로토콜은 다음과 같음

  - NFS (Network File System)
  - SMB/CIFS (Server Message Block/Common Internet File System)
  - FTP (File Transfer Protocol)
  - HTTP (Hypertext Transfer Protocol)
  - AFP (Apple Filing Protocol)
![image](https://user-images.githubusercontent.com/20410193/147643372-5729b660-ba59-41e2-b87e-69e2999c0a98.png)
# 출처

- [스토리지 기초 지식 1편: DAS, SAN 그리고 NAS](https://tech.gluesys.com/blog/2019/12/02/storage_1_intro.html)
- [SAN(Storage Area Network)](https://www.vmware.com/kr/topics/glossary/content/storage-area-network-san.html)
