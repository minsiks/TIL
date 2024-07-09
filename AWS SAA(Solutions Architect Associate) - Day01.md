# 24/7/9 AWS SAA(Solutions Architect Associate) - Day01

## AWS 시작하기

### 1. AWS 클라우드 개요

- AWS Regions
  - 데이터 센터의 집합
  - 리전에 따른 차이
    - 법률
    - 지연 시간
      - 사용자들이 가까운 지역 
    - 가능한 서비스
      - 리전마다 가능한 서비스 다름
    - 요금
      - 리전마다 요금이 다름
- AWS 가용 영역
  - 리전안에 많은 가용 영역 존재
    - 보통 3개씩
      - ex) 호주 ap-southeast-2a /ap-southeast-2b/ ap-southeast-2c
    - 최소 3개, 최대 6개
  - 각각의 가용 영역은 여분의 전원 네트워킹, 통신 기능을 갖춘 하나 또는 두개의 개별적인 데이터 센터로 구성
  - 각가의 가용 영역은 별개
    - 장애가 났을때도 영향을 받지 않음
  - 높은 대역폭의 초저지연 네트워킹으로 연결
- AWS 데이터 센터
- AWS 엣지 로케이션 / 전송 지점
  - 400개이상의 전송지점을 가지고 있음
  - 지연을 최소화하기 위함
- AWS 콘솔
  - Global Services
    - IAM
    - Route 53
    - CloudFront
    - WAF (Web Application Firewall)
  - Region-scoped
    - EC2
    - Elastic Beanstalk
    - Lamda

### 2. IAM & AWS CLI

#### IAM

- Identity and Access Management
- Global Service
- Root account : 오직 계정을 사용할 때 사용, 사용하거나 공유하면 X
- Users : 조직안에 하나의 사용자, 묶을수도 있음
- Groups : 사용자만 배치 가능
- 어디에도 속하지 않은 user도 가능
- 두개의 서로 다른 그룹에 속한 유저도 가능

- user가 허용 필요 시 권한을 부여
  - Users or Groups은 Json문서를 지정할 수 있음
  - 모든 사용자에게 권한을 부여하지 않음
  - 최소 권한의 원칙을 유지

- IAM 서비스를 사용하여 AWS에서 사용자를 생성하는 연습

- IAM Policies
  - 그룹레벨에서 정책을 정하면 해당 그룹의 정책이 해당 그룹 유저에게 적용
  - 그룹이 없는 User에게는 인라인 정책을 생성 가능
  - 중복그룹이 있는 유저에게는 그룹의 정책들이 모두 적용
  - 정책은 JSON으로 구성
    - Version : 버전 /  ex) "2012-01-17"
    - id : 해당 정책의 id (optional)
    - Statement : 하나 또는 여러개
      - Sid : statement의 식별자 (optional)
      - Effect : 특정 API에 접근하는 걸 허용할지 거부할지지에 대한 내용 / "Allow" or "Deny"
      - Principal : 특정 정책이 적용될 사용자. 계정, 역할
      - Action : effect에 기반해 허용 및 거부되는 API 호출의 목록
      - Resource : 적용될 action의 리소스의 목록
      - Condition : statement가 언제 적용될지를 결정(optional)

- IAM Password Policy
  - 비밀번호 정책
  - AWS에서 비밀번호 정책을 구성할 수 있다.
    - 최소 숫자길이
    - 특정한 문자 (예/ 대문자, 소문자,숫자,비문자) 요구
  - 사용자의 비밀번호 변경 허용/금지
  - 일정 기간 후 사용자 비밀번호 만류 -> 새 비밀번호 설정 요구
- 다요소 인증 (MFA-Mutil Factor Authentication)
  - 적어도 Root 계정을 보호
  - 전체 IAM유 저를 보호하기 위함
  - MFA = 비밀번호 + 보안장치 함께 사용
    - EX_ Password+ MFA Tocken
  - MFA 종류
    - Virtual MFA device
      - google Authenticator(phone only)
      - Authy (multi-device)
        - 하나의 장치에 원하는 만큼의 계정및 사용자 등록 가능
    - U2F 보안 키
      - 물리적 장치
        - Yubikey by Yubico (3rd party)
          - 하나의 키로 여러가지의 계정/루트 계정을 사용 가능
    - Hardware Key Fob MFA Device
      - Provided by Gemalto (3rd party)
    - Hardware Key Fob MFA Device for AWS GovCloud(US)
      - Provided by SurePassID (3rd party)
- MFA 실습

#### AWS CLI

> AWS에 접근하기 위한 3가지 Options
>
> - AWS Management Console (protected by password + MFA)
> - AWS Command Line Interface (CLI) : protected by access keys
> - AWS Software Developer Kit (SDK) - protected by access keys
>
> AWS 콘솔에서 엑세스 키 생성
>
> 유저들이 관리
>
> 비밀번호와 같이 암호와 같음
>
> AccessKey ~= username
>
> Secret Access Key ~= password

- 명령줄로 AWS을 접근
- AWS SDK란?
  - 소프트웨어 개발 키트
  - 특정 언어로된 라이브러리 집합
  - 프로그래밍 언어에 따라 개별 SDK가 존재
  - 코딩을 통해 애플리케이션 내에 심어 두어 AWS services를 접근 할수 있다.

#### IAM Roles for Services

- AWS 서비스를 의해 사용되도록 만들어진 역할 != USERS
- Ex) EC2 인스턴스(가상 서버)를 만들 때 IAM Role을 하나의 Entity로 구성
- Common roles
  - EC2 Instance Roles
  - Lamda Function Roles
  - Roles for CloudFormation

#### IAM Security Tools

- IAM 보안 도구
  - IAM Credentials Report (account-level)
    - 보고서는 계정에 있는 사용자와 다양한 자격 증명 상태를 나타냄
    - 자격 증명 보고서 -> csv로 다운 
      - 비밀번호가 언제 설정되었는지, mfa를 사용하는지, 액세스키를 언제설정되었는지 등등
  - IAM Access Advisor (user-level)
    - 액세스 관리자는 사용자에게 부여된 서비스의 권한과 해당 서비스에 마지막으로 액세스한 시간 표시
      - 사용자 - 액세스 관리자
        - 최근 4시간 동안 활동 내역 표시 

- 루트 계정은 AWS계정을 설정할 때를 제외하고 사용 X
- 그룹을 사용하여 관리

- 강력한 비밀번호
- 가능하면 MFA사용
- ROLE을 사용하세요
- CLI/SDK 사용하면 Acess Key 사용
- 액세스 관리자, 자격 증명 보고서를 사용하여 권한과 활동내역을 활용
- Access key나 사용자를 공유 x

요약

​	

