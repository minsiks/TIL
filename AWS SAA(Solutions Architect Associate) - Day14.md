# 24/7/26 AWS SAA(Solutions Architect Associate) - Day12

## 재해복구

### 1. 재해복구

- 온프레미스간 재해복구
- 온프레미스 -> 클라우드 : 하이브리드 복구
- 모두 클라우드 : 완전 클라우드 유형
- 핵심 용어
  - RPO :  즉 복구 시점 목표
  - RTO :  재해 복구시 시점 애플리케이션 다운타임
- 복구 전략
  - 백업 및 복구
    - high rpo
    - 시간이 오래걸리고  rto도 커지지만 값이 저렴
    - 쉽고 비용이 저렵한데 rpo rto 가 높음
  - 파일럿 라이트
    - 애플리케이션이 축소버전에서 계속 실행
    - 복구할때 여타 시스템만 추가
    - 크르티컬 시스템 보조
  - 웜대기
    - 시스템 전체를 실행하되 최소한의 규모로 가동
    - 프로덕션 로드로 재해시 저장
  - 다중사이트 혹은 핫 사이트
    - 몇분 몇초로 rpo rto 하지만
    - 비쌈
    - 이미 실행중인 사이트를 
    - active - active 
  - 클라우드시
    - 멀티 리전

#### 데이터베이스 마이그레이션

- 온플레미스 -> AWS Cloud
- DMS 사용 해야함 (Database Migration Servcie)
  - 동종 마이그레이션도되고
  - 이종 마이그레이션 가능
  - CDC Change Data Capture를 이용한 지속적 데이터 복제도 가능
  - ec만들어야만 함
  - 소스는 데이터베이스에 될 수 있음
    - Azure도 됨
    - amzon RD
    - S3
    - Document
  - 타겟도 여러가지 됨
- SCT
  - 소스 데이터베이스와 타깃 데이터베이스의 엔진이 다른 경우 사용

#### Aurora MySQL로 마이그레이션

- 스냅샷을 생성 스냅샷을 데이스베이스의 복원
  - 다운타임 생김
- 읽기 전용 복제본을 mysql로 생성
  - 시간이 많이 걸리고 비용이 발생할 수있음
- percona xbackup사용
- mysqldump를 데이터베이스에서 실행해서 aourora로 보냄
- 가동된체로 지속적인 복제
- postgresql도 거의 동일

#### 온프레미스 전략 클라우드에서

- ami 다운로드가능
- vm 가져오기/ 내보내기
- AWS 애플리케이션 Discovery Service
  - 대량 마이그레이션 유용
  - migration Hub 필요
- DMS
  - 복제를 허용 상호간
- SMS 
  - 라이브서버를 볼륨을 직접 복제 

#### AWS Backup

- 완전 관리형 서비스
- 서비스간에 백업을 중점적으로 관리
- 스크립트 없이 백업전략으로 선택 가능
- 대부분의 아마존 서비스 가능
- 리전 간 백업을 지원
- 계정간 백업도 지원
- 지정 시간 복구를 지원
- 예약된 백업
- 태그가 지정된 리소스 백업 가능
- 백업 플랜도 만듬
- 볼트 잠금
  - WORM 정책 시행시 백업 볼트에 저장한 백업을 삭제할 수 없음
  - 백업에대한 추가 방어막
    - 축소 또는 변경 방어
  - 백업의 안정성 보장

#### Application Discovery Service

- 서버를 클라우드로 마이그레이션
- 두가지 유형
  - Agentless Discovery
    - 가상머신, 구성, CPU와 메모리 및 디스크 사용량과같은 성능 기록에 대한 정보제공
  - Agent-absed Discovery
    - 가상 머신 내에서 더 많은 업데이트와 정보 얻을 수 있음
    - 시스템구성, 성능, 실행중인 프로세스, 시스템 사이의 네트워크 연결 등에대항 정보제공
- 모든 결과를 aws migration hub에서 볼 수 잇음
- MGN
  - 온프레미스->AWS 가장 간단
  - 리호스팅 가능

- 대규모 데이터 AWS로 전송
  - 200TB를 한다면
    - 공용인터넷 / SITE-TO-SITE
      - 즉시 셋업
      - 185일정도 걸림 반년
    - Direct connect
      - 초기 설치에 시간이 오래걸림
      - 10배정도 빠름
    - Snowball
      - 23 퍼실리티에 페럴럴로 하면
      - 종단 간 전송에 일주일
- VMware Cloud on AWS
  - 온프레미스 클라우드 같이 사용 할때 사용

### 2. 더 많은 솔루션 아키텍쳐

#### HPC

- 고성능 컴퓨팅

  - 클라우드에선 많은 리소스를 즉각적으로 만들수있어서 최적화
  - 필요에따라 증가 가능하기 때문에
  - 필요없으면 삭제해서 비용x

  - 사용사례
    - 머신러닝, 분석
  - hpc의 작업을 돕는 aws 서비스
    - 데이터 매니지먼트 & 트랜스퍼
      - AWS Direct Connect
        - 초당 GB 속도로 프라이빗 보안 네트워크를 통해 전송
      - Snowball & Snowmobile
        - 물리적 라우팅을 통해 클라우드로 pb 단위 데이터를 옮길 때 사용
      - AWS DataSync
  - 컴퓨팅 네트워킹
    - EC2 인스턴스
    - EC2 SR-IOV
      - Enhanced Networking
      - 더 넓은 대역폭 더 좋은거
      - Elastic Network Adapter (ENA) : 네트워크 속도를 100기가까지 올려줌
      - intel 82599 VF 를 사용
    - Elastic Fabric Adapter (EFA)
      - 고성능 컴퓨팅을 위한 ENA
      - 오직 리눅스
      - 노드간 소통이나 밀집된 워크 로드 처리에 좋음
      - MPI가 표준
      - Linux에서 제공하는 Btpass를 이용하여 유용
  - Storage
    - 인스턴스가 연결된 스토리지
      - EBS
      - Instance Store
    - Network storage
      - S3
      - EFS
      - FSx for Lustre
        - HPC를 위한
  - 자동화 및 오케스트레이션
    - AWS Batch
      - 작업 예약과 실행이 쉬워짐
    - AWS ParallelCluster
      - 오픈 소스 클러스터 관리 서비스
      - 텍스트 파일로 구성해 aws 배포
      - EFA와 같이 사용

### 3. 기타 서비스

#### 대규모 인프라 배포 및 관리 섹션

- CloudFormation
  - 리소스에 대해 인프라의 윤곽을 구분짓는 선언적 방법
  - 대부분의 리소스가 지원
  - 장점
    - 모든 인프라가 코드
    - 수동으로 리소스를 만들필요 x
    - 비용적
      - 리소스 예측 쉽고
      - 쌈
  - 생산성
    - 그때그때 파괴 생성 가능
    - 다이어그램 만ㄷ르수 있음
    - 선언적 프로그래밍 가능
  - 하나하나 안만들어도됨
    - 문서 활용가능
  - 보안
    - 서비스 역할을 사용할 수있음
    - 서비스 역할을 사용해야함
    - IAM권한
    - 사용사례
      - 최소 권한 원칙
- SES (Simple Email Service)
  - 완전관리 서비스
  - smtp 서버를 사용하면 대량의 이용자에게 보냄
  - 이메일을 스팸으로 봤는지도 볼 수있음
  - 통계도 볼수있고
  - 최신 보안 기준 DKIM SPF
  - API는 콘솔에서 액세스
- Pinpoint
  - 확장 가능한 양방향 마케닝 커뮤니케이션 서비스
  - sms,email등
  - 메시지를 세분화하고 개인화 할 수 있음
  - 답장도 받을 수 있음
  - 성공하면 log로 전달
  - sns vs ses
    - pinpoint는 직접 메시지를 전달 하고 관리 가능

- 시스템 관리자 SSM session- manager
  - EC2 인스턴스와 온프레미스 서버에서 보안 셸을 시작 가능
  - SSH 액세스, 베스턴 필요 X
  - SSH 22 포트 필요 없음
- SSM 기타서비스
  - Run Command
    - 실행 문서를 실행하는데 사용
    - 이 명령들을 필요할 때 ssh 필요x
    - 실행된 결과 로그로 찍힘
    - 실패하면 sns로 감
    - iam과 완벽하게 통합
  - 패치 매니저
    - 관리과정을 패칭하는데 사용
    - 보안업데이트
    - 보고서
  - 유지 관리 기간
    - 작업을 수행할 일정을 정의
    - os 패치, 설치 등
    - 언제 트리거
    - 스케줄
    - 실행될 작업
  - 자동화
    - 배포작업 및 관리 이지하게
    - 자동화 런북 존재
- cost Eplorer
  - 비용 관련
  - 전체적인 데이터 분석
  - 비용탐색기
  - 최적의 절감형 서비스 플랜 선택할 수있음
- 비용이상 탐지
  - 머신 러닝으로 감지
  - 이상 탐지해서 알림보낼것
- 아마존 배치
  - 완전관리형 배치 처리 서비스
  - 수십만개의 컴퓨팅 잡을 실행
  - 시작과 끝이 있는 작업을 실행
  - 동적으로 ec2인스턴스 스팟인스ㅓㄴ스로 실행
  - 적절한 양의 컴퓨팅 메모리 구성
  - 도커 이미지와 ecs 서비스에서 실행되는 것
  - 적절한 수를 자동으로 조절
  - 비용이 최적화
- Appflow
  - SaaS 애플리케이션 및 AWS 사이에 데이터를 전송 할 수 있는 완전 관리형 통합 서비스
  - 소스 : Salesforce,SAP 등
  - 목적: S3, Redshift 등
  - 일정에 따라 또는 특정 이벤트 응답, 주문형 다양하게
  - 필터링 유혀성과같은 데이터 변환가능
  - 암호화 가능
  - 통합을 만드는데 시간 안써도됨
- aws 증폭
  - 웹 및 모바일 애플리케이션 개발 도구
  - 많은 스택을 한곳에서 통합
  - 웹 애플리케이션 빌드 가능

### 4.Well Architected 백서

- 기본 가이드
  - 오토스케일링 사용 - 필요용량 추측 x
  - 자동화를 통해 아키텍처를 쉽게
  - 진화하는 아키텍처를 만들어야함
  - 데이터로 아키텍처를 구성
  - Gameday를 통해 성장
- Well Architected 백서
  - 운영 우수성
  - 보안
  - 안정성
  - 성능 효율성
  - 비용 최적화
  - 지속 가능성

- 트러스터 어드바이저
  - 계쩡에 대한 높음수준의  계정 평가를 제공
  - 6가지 범주로 분류
    - 비용최적화
    - 성능
    - 보안
    - 내결함섬
    - 서비스제한
    - 운영 우수성
  - 비지니스 & 엔터프라이즈 지원 플랜
    - 프로그래망 방식 액세스 제안

- 아키텍처 패턴

### 4. 시험
