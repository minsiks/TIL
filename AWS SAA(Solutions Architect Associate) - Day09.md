# 24/7/19 AWS SAA(Solutions Architect Associate) - Day08

## 컨테이너

### 1. 컨테이너 섹션

- ECS 서비스 오토 스케일링
  - 지표
    - CPU 사용률
    - 메모리 사용률
    - ALB 관련 지표, 타겟당 요청수
- 대상 추적 스케일링
- 스텝 스케일링
- 예약 스케일링
- 스케일링과  EC2 인스턴스 클러스터의 확장과는 다름
- EC2가 없다면 Fargate 사용 서버리스기 때문
- EC2런치타입 오토스케일링
  - 오토 스케일링 그룹
    - CPU 사용률에
  - ECS 클러스터 용량 공급자
    - 새 태스크를 실행한 용량이 부족하면 자동으로 ASG확장
    - RAM이나 CPU 모자랄 때 EC2 인스턴스 추가

#### Amazon ECR

- ECR = Elastic Container Registry
- 도커 이미지를 저장하고 관리
- ECS와 완전히 통합 , 백그라운드에서 S3로

#### Amazon EKS

- Elastic Kubernetes Service
- 관리형 Kubernates 클러스터를 실행 가능
- Kubernates
  - 오픈 소스 시스템
  - 도커로 컨테이너화한 애플리케이션의 자동 배포, 확장, 관리를 지원
- ECS와 비슷, 사용하는 API가 다름
- 두가지 실행모드
  - EC2 시작모드
  - Fargate모드
- 사용사례
  - Kubernetes관련 서비스를 사용 시
- Kubernetes는 클라우드 애그노스틱으로 Azure, Google Cloud등 모든 클라우드에서 지원

- 노드유형
  - 관리형 노드 그룹
    - aws로 노드 ,즉 ec2인스턴스를 생성하고 관리
    - 오토 스케일팅 그룹의 일봉
  - 자체 관리형 노드
    - 직접 노드 생성 asg로 일부 생성
  - Fargate 모드
    - 유지 관리도 필요 없고 노드관리 안해도됨
- 데이터 볼륨
  - 스토리지 클래스 매니페스트를 지정해야함
  - 컨테이너 스토리지 인터페이스(CSI)라는 규격 드라이버 활용

#### AWS App Runner

- 완전 관리형 서비스로 규모에 따라 웹 어플리케이션, API배포를 도움
- 누구나 aws에 배포가능
- 웹 애플리케이션이나 API에 들어갈 기본 설정을 설정
- 자동 작업
