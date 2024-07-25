# 24/7/25 AWS SAA(Solutions Architect Associate) - Day11

## 머신러닝, 모니터링 및 감사

### 1.머신러닝

#### Amazon Comperhend

- 자연어를 처리하는 NLP 서비스
- 완전 관리형 서버리스 서비스
- 머신러닝
  - 텍스트 언어를 이해
  - 주요문구장소 살람등 분석
- 사용사례
  - 고객 이메일
  - 분석한내용으로 문서 생성 가능

#### Comprehend Medical

- 비정형 의료 텍스트에서 유용한 정보를 반환
  - 의사소견서 등
- S3에 문서 저장

#### SagaMaker

- 완전 관리형 서비스
- 머신 러닝 모델을 구축하는 개발자와 데이터 과학자를 위한 서비스

#### Forecast

- 완전 관리형 서비스
- 머신 러닝을 사용하여 매우 정확한 예측을 제공
- 제품 수요계획 등

#### Kendra

- 완전관리형 문서 관리 서비스
- 문서 검색 서비스

#### Personalize

- 완전 머신 러닝 서비스
- 실시간 맞춤화 추천으로 애플리케이션을 구축
  - 추천

#### Textract

- 텍스트를 추출

### 2. 모니터링 및 감사

#### CloudWatch Metircs

- CloudWatch는 모든 서비스에 대한 지표 제공
- 지표(Metric)는 변수
- 지표는 이름공간에 속함
- 측정 기준 : 지표의 속성
- 지표당 최대 측정기준 30개
- 태임스탬프가 꼭 이썽야함
- 스트림
  - 지표를 원하는 대상으로 지속적으로 스트리밍하면 실시간으로 전송, 지연시간 짧아짐
    - Kinesis Data Firehose

#### CloudWatch Logs

- 애플리케이션 로그를 저장하는곳
- 로그그룹 
- 로그 스트림 : 로그인스턴스나 로그 파일이 있는 컨테이너
- 로그 만료 정책을 정의
- 다양한 대상에도 전송 가능
  - s3
  - 등
- 암호화
- sdk,CloudWatch Logs Agent, CloudWatch Unitied Agent 전송가능

- Elastic Beanstalk : 애플리케이션과 연결 가능
- ECS : 컨테이너에서 클라우드 워치로 보냄
- 람다 함수자체에서 전송
- VPC
- API Gateway
- Route S3
- 로그 인사이트
  - 로그를 시각화함
  - 검색하고 분석가능
  - 샘플쿼리 많음
  - 쿼리 언어를 제공
    - 필터링 계산 정렬 다 가능
  - 대시보드에 추가가능
  - 실시간 엔진이 아니라 쿼리 엔진
- S3 Export
  - 완료 될때까지 최장 12시간까지
  - CreateExport Task
  - 근 실시간이나 실시간이 아님
- CloudWatch Logs Subscription
  - 로그 이벤트들의 실시간 스트림을 얻고, 처리 분석 가능
  - 다양한 곳에 보낼 수있음
  - 필터로 필터링 가능

- CloudWatch Logs for EC2
  - 기본적으로는 어떤 로그도 옮기지지 않음
  - EC인스턴스에 에이전트라는 프로그램 실행 로그파일 푸시
  - IAM 역할로 열어줘야함
  - 온프레미스 환경에서도 셋업 가능
- 통합 CloudWatch 에이전트
  - 온프라미스 ec2 버전 다 가능
  - CloudWatch Logs Agent
    - 좀더 오래된 버전
    - 로그만 보냄
  - 통합 에이전트
    - 프로세스나 램 같은 추가적인 시스템 단계 지표 보냄
    - 로그도 보냄
    - SSM 파라미터로 에이전테를 쉽게 구성
  - 지표
    - cpu
    - disk
    - ram
    - netstat
    - processes
    - swp space

- Alarms
  - 매트릭에서 나오는 알람을 트리거하는데 사용
  - 샘플링,퍼센티지,최대,최소등 옵션이있음
  - 상태
    - ok
    - insufficeint_data
    - arlaam
  - 기간
  - 타겟
    - EC2
    - 오토스케일링
    - SNS 서비스
  - 복합 알람
    - 다수의 매트릭이랑 사용
    - AND OR 조건
    - 알람 노이즈를 줄이는데 유용
  - 복구
    - 상태 체크
- EventBridge
  - CRON 작업을 예약 가능 (스크립트 예약 가능)
  - 이벤트 패턴에 반응가능
    - 예 ) 로그인
  - LAMDA 트리거 가능
  - JSON 형태
  - 스키마 레지스트리 라는것이 있다
    - 버스 이벤트를 분석
  - 스키마를 버저닝 할수도있음
  - 리소스 기반 정책
    - 특정 이벤트 버스의 권한을 관리 가능

- Insight 유형
  - Container Insighs
    - 컨테이너로부터 지표와 로그를 수집, 집계함
    - 사용가능
      - ECS
      - EKS
      - EC2
      - FARGATE
  - Lambda insights
    - 람다에서 실행되는 서버리스 애플리케이션을 위한 모니터링과 트러블 슈팅 솔루션
    - CPU 시간, 메모리 디스크, 네트워크 등 지표를 집계 요약
    - 함수의 선능을 모니터링
  - Contributor Insights
    - 기고자 데이터를 표시하는 시계열 데이터 생성 로그를 분석
    - 네트워크 성능에 영향을 미치는 사용자 찾을 수 있음
    - VPC 로그, DNS 로그 등 모든 로그에서 작동
    - 불량 호스트를 식별 가능

