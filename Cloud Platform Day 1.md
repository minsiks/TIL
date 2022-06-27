# 6/27 Cloud Platform Day 1

> Final Project 전 배울 것들
>
> 1. NCP(Naver Cloud Platform)
>    1. CLOUD
>       - IAAS - Linux OS, JDK, Mysql, Web Server
>       - PAAS - Naver AI Platform
>       - SAAS
>
>    

## NCP(Naver Cloud Platform)

### CLOUD

- IAAS - Linux OS, JDK, Mysql, Web Server
  - Linux OS는 개별적 공부가 필요.
  - centOS
- PAAS - Naver AI Platform

### [NCP] 접속 가이드

https://cafe.naver.com/2022webservice

[API 가이드](https://api.ncloud-docs.com/docs/ko/home)

#### id부여 (2조) 비밀번호:multi0623! 

- ai14-3 - 1번 서버 v -김민식
  - 비밀번호 변경 multi14!!
- ai14-4 - 1번 서버
- ai14-5 - 2번 서버
- ai14-6 - 2번 서버

#### 1. 로그인 후 콘솔로 이동

#### 2. Platform에 Classic으로 변경 후 Service - Server - 서버생성 - 서버 설정

- Compact 선택
- centos-7.2-64 다음

#### 3. 인증키 설정

- 새로운 인증키 생성
  - multiminsikkey

#### 4. 네트워크 접근 설정

- 신규 ACG 생성 (Access Control Group)
  - ACG 이름 minsikhome
  - 접근소스 0.0.0.0/0 - 모든 접근 허용
  - 허용포트 1-65535 생성 - 모든 포트 허용
  - 서버에 OS설치를 하는것이다.

- Server
  - 인증키와 ACG로 내서버 확인
  - 상태가 운영중으로 바뀔때 까지 대기
  - 체크 후 포트 포워딩 설정
    - 서버 접속용 공인 IP
      - 106.10.58.9
      - SSH 보안 프로토콜을 이용하여 접속하기 위한 공인 IP
    - 외부 포트 65500
    - 추가
    - 터미널 프로그램(Putty) 다운로드
      - 64-bit x86 다운로드
- Putty 실행

> 아마존 웹 서비스 (AWS)도 지금과 비슷한 진행 방법으로 생성 가능