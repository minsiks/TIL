# 24/7/19 AWS SAA(Solutions Architect Associate) - Day08

## 디커플링 애플리케이션, 컨테이너

### 1. 디커플링 애플리케이션

#### SNS + SQS : Fan Out

- SNS 토픽으로 한 번 메시지를 전송하고 원하는 만큼 SQS 대기열을 SNS 토픽에 구독시키는 것
- 완벽하게 분리, 데이터 손실 X
- SQS로 데이터 지속성, 지연 처리, 작업 재시도 등의 효과
- 시간이 지날수록 SQS 대기열을 SNS토픽의 구독자로 추가가능
- SQS 대기열 정책을 SNS토픽이 SQS 대기열에 쓰기 할 수 있도록 허용 필수
- 교차리저냅선 한 리전의 SNS 토픽이 다른 리전의 SQS대기열에 메시지를 보내는것이 가능 (보안 허용시)

- Application 예
  - S3 이벤트를 여러 개의 대기열로 보낼 때 가정
  - Kinesis Data Firehost를 통해 SNS에서 Amazon S3로 직접 데이터를 보내기 가능
  - Amazon SNS - FIFO 기능

#### Kinesis

- 실시간 스트리밍 데이터를 손쉽게 수집하고 처리, 분석 가능
- Kinesis Data Stream : 데이터 스트림을 수집하여 처리
- Kinesis Data Firehose : AWS 내부나 외부의 데이터 저장소로 읽음
- Kieesis Data Analytics : SQL 언어나 Apache Filink를 활용하여 데이터스트림을 분석
- Kenesis Video Stream : 비디오 스트림을 수집하고 처리하여 저장

#### Kinesis Data Streams

- 시스템에서 큰 규모의 데이터 흐름을 다루는 서비스
  - 여러개의 샤드가 존재
  - 데이터는 모든 샤드에 분배
  - 데이터 수집률이나 소비율 측면에서 스트림의 용량을 결정
- 보존기간은 1일에서 365일 사이
- 데이터를 다시 처리하거나 확인 가능
- 데이터가 Kenesis로 들어오면 삭제 불가 (불변성)
- 데이터 스트림으로 메시지를 전송하면 파티션 키가 추가, 파티션 키가 같은 메시지들은 같은 샤드로 들어가게 되어 정렬
- 리얼타임
- SDK,KPLKensis Agent로 전송
- 유형
  - 프로비저닝 모드
    - 프로비저닝할 샤드 수를 정하고 API를 활용하거나 수동으로 조정
  - 온디맨드
    - 프로비저닝 할 필요가 없고 알아서 용량 조장
    - 30일 동안 관측한 최대 처리량으로 조정
    - 비용 산정 다름
- 보안
  - IAM Policies
  - HTTPS
  - KMS
  - 암호화/복호화 데이터
  - VPC 엔드포인트

#### Kinesis Data Firehose

- Producers로 부터 데이터를 가져올 수 있는 서비스
- 대상
  - AWS 대상
    - Amazon S3
    - Amazon Redshift : 웨어하우징 데이터베이스
    - Amazon OpenSearch
  - 제3자 파트너 대상자
  - 자신의 API
    - HTTP Endpoint
- 백업
  - s3
- 완전관리형 서비스
- Firehose를 통과하는 데이터에 대해서만 비용 지불
- 근 실시간
- 여러 데이터 포맷, 압축,
- 람다 가능
- 오토스케일링 있음

#### Kinesis 정렬 

- 파티션키를 가지고 있는 같은 샤드로 언제나 이동

#### sqs 정렬

- 표준방식에는 순서가 없음
- fifo는 선입선출
  - 기본적으로 소비자가 하나
  - 만약 소비자 숫자를 스케일링하고 서로 연관된 메시지를 그룹화 하려는 경우 그룹 ID를 이용

#### Amazon MQ

- 온프레미스에서 기존 애플리케이션을 실행하는 경우, 마이그레이션 하는 경우 개방형 프로토콜인 MQTT, AMQP, STOMP, WSS, Openwire 등을 사용 원할 때 Amazon MQ 사용
- 아래것들을 위한 관리형 메시지 브로커 서비스
  - RabbitMQ
  - ActiveMQ
- 무한 확장이 가능하지 않음
- 서버에서 실행되므로 서버 문제가 있을 수 있기에 다중 az실행가능
- sqs보이는 대기열, sns처럼 보이는 주제 기능을 단일 브로커의 일부로 제공

### 2. 컨테이너 섹션

#### 도커

- 앱 배포를 위한 소프트웨어 개발 플랫폼
- 컨테이너 기술
- 아무운영체제 사용가능
  - 아무 머신
  - 호환성 문제 x
  - 애니애니
- 도커에 사용사례
  - 마이크러 서비스 아키텍처

- 도커리파지토리에 도커 이미지 있음
- Docker Hub
  - 퍼블릭 리포지토리
  - 많은 기술에 맞는 기본 이미지를 찾을 수 있다
- Amazon ECR (Amazon Elastic Confainer Registry)
  - 프라이빗 리파지토리
  - 비공개이미지
    - 퍼블리 리파지토리 옵션있음
- 순전히 가상화 기술은 아님
- 리소스가 호스트와 공유, 한 서버에 다수의 컨테이너를 공유 가능
- AWS 도커 컨테이너 관리
  - ECS 
  - EKS : Kubernets
  - Fargate
    - ECS, EKS 둘다 함께 작동가능
  - ECR
    - 컨테이너 이미지 저장

### Amazon ECS

- EC2 Launch 타입

  - 도커 컨테이너를 AWS에 런치하면 ECS 클러스터에 ECS 태스크를 실행하는 것
  - EC2 Launch Type을 선택하면 EC2 인스턴스가 들어있음
  - 인프라를 직접 프로비저닝 해야만함
  - 각각 ECS 에이전트를 실행해야 함
  - 클러스터 등록
  - 태스크를 수행하면 컨테이너가 시작하거나 멈춤

- EC2 Fargate 타입

  - 인프라 프로비저닝 x,  
  - 서버리스
  - 태스크 정의만 생성
  - 필요한 cpu나 ram에 따라 생성
  - 확장하려면 태스크 수만 늘림

- ECS의 IAM 역할

  - ec2런치 타입일때
    - ecs 에이전트만이 ec2인스턴스 프로파일을 사용
    - api 호출
    - CloudWatch 로그에 API 호출
    - ECR로부터 도커이미지 가져옴
  - ECS 태스크 역할
    - 각자의 특정 역할 만들기 가능
    - 역할이 각자 다른 ECS 서비스에 연결할 수 있게 위함
    - ECS 서비스의 태스크 정의에서 태스크 역할을 정의

- ECS 로드 밸런서 통합

  - ALB 좋은 옵션
  - NBL 처리량이 매우많거나 그럴때 사용
  - ELB는 사용할수있지만 권장 X

- ECS 데이터 볼륨 (EFS)

  
