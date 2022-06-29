# 6/28 Cloud Platform Day 02

> Final Project 전 배울 것들
>
> 1. NCP(Naver Cloud Platform)
>    1. CLOUD
>       - IAAS - Linux OS, JDK, Mysql, Web Server
>       - PAAS - Naver AI Platform (챗봇, 이미지 해독기 **등등**)
>       - SAAS
>
> [4조 (padlet.com)](https://padlet.com/tidnjrk010/obsul80ccbcf9wkt)
>
> [[GitHub\] 깃허브 토큰(Token) 생성하는 법 :: 대두코기 (tistory.com)](https://hoohaha.tistory.com/37)

## [NCP] 접속 가이드

### 4.네트워크 접근 설정

#### Server

- 인증키와 ACG로 내서버 확인
- 상태가 운영중으로 바뀔때 까지 대기
- 체크 후 포트 포워딩 설정
  - 서버 접속용 공인 IP
    - 106.10.58.9
    - SSH 보안 프로토콜을 이용하여 접속하기 위한 공인 IP
  - 외부 포트 60003
  - 추가
  - 터미널 프로그램(Putty) 다운로드
    - 64-bit x86 다운로드

- 서버접속하기위해서는 pem(인증키)와 서버공인 ip가 필요

- 서버 관리 및 설정 변경 -> 서버에 접속 가이드
- 서버 관리 및 설정 변호 -> 관리자 비밀번호 확인
  - 서버 이름 : multi14관리자 이름 : root / 비밀번호 : B6iYn8*3eMLgy
  - passwd치면 패스워드 변경
  - pwd : multi14!!



#### Final 조 구성 (4조)

1. 안원영, 정세연, 장효준 , 김민식

- Idea!

  - 구인구직 서비스 플랫폼을 응용한 봉사활동 모집/참여  통합 및 다양한 이벤트 배치
  - 실제 봉사활동 모집공고들을 나열시키고 정부의 공식적인 홈페이지와 다른 세련된 UI를 제공
  - 지역별 / 지도 상 구분 지어 동적 이미지화시켜 보여준다

### CentOS 명령어 정리
#### 자동완성 Tap

#### vi편집기 실행

vi test.txt 

test.txt 파일이 있으면 실행되고, 없으면 새로 만들어서 실행된다.

[vi 명령어](https://www.notion.so/96d921122e3f4fd2b36064e3b0d2353a)

w : write

q : quit

****

**파일 내용 확인**

cat  "파일 이름.파일형식"

****

**파일 및 디렉토리**

****

**디렉토리 변경**

\# 절대 패스

cd /home/student 

\# 상대 패스

cd../home/student 

\# 현재 작업 디렉토리의 위치와 상관없이 홈 디렉토리로 이동

cd  



****

**현재 작업 디렉토리 절대 패스로 출력**

pwd

****

**디렉토리 파일목록 출력**



\# 간략히 보기

ls "디렉토리명"

\# 자세히 보기

ls -l 

\# 숨김파일 보기(.으로 시작하는 파일)

ls -a 

\# 숨김파일까지 자세히 보기

ls -al  



-al 로 표시되는 파일은

파일 종류, 접근권한, 링크수, 소유자, 소유그룹, 파일크기, 마지막 수정일시, 숨김파일등 으로 나온다.



[파일 종류](https://www.notion.so/0b1699b0b7b6437f891e216e2c25c860)

**디렉토리 생성 및 삭제**

\# 생성

mkdir  "디렉토리 이름"

\# 삭제 - 디렉토리 안에 다른 파일이나 디렉토리가 존재하지 않아야만 가능 

rmdir "디렉토리 이름"

****

****

**파일 복사, 삭제, 파일 및 디렉토리 이동**

\# 복사

cp  "파일명.확장자"  "(복사한)파일명.확장자" 

\# 삭제

rm  "파일명.확장자"

\# 이동

mv  "파일명.확장자"  "이동할 경로

"/"(변경가능)파일명.확장자"

mv "이동할 대상"  "디렉토리 이름"



\# 이동할 대상의 이름은 그대로 유지

****

****

**file 종류 표시**

file 대상파일경로(혹은 파일경로패턴)

****

**파일 찾기**

find 검색시작위치 -name "파일명패턴"# 특정 파일명 패턴을 갖는 파일들을 검색해서 지우기find 검색시작위치 -name "파일명패턴" -delete

****

**파일 내려받기**

wget 다운로드URL 

이상하게 저장되는 경우가 있어서 다음 명령어 사용 많이함

**다른 이름으로 저장하기**

wget -O 저장될 파일이름 다운로드URL

**이어받기**

wget -c 다운로드URL

**압축하기**

tar zcvf 생성될압축파일이름 압축할원본파일_혹은_디렉토리

**해제하기**

tar zxvf 압축파일_이름

c : 새로운 묶음 만듬

x : 묶인 파일 풀어줌

f : 묶음 파일의 이름 지정 옵션

v : 묶음 파일을 풀거나 묶을 때 과정을 화면에 출력

**zip 명령어 압축/해제**

zip 압축파일이름 압축대상파일이름 

unzip 파일이름

gzip 명령어와 달리 묶음 파일을 거치지 않고 바로 압축/해제 가능

****

### 1.[CentOS] JDK1.8 설치
```
yum  list  java*jdk-devel yum install  java-1.8.0-openjdk-devel.x86_64 java -version 
```

### 2.[CentOS] Mysql8.0 설치

```
1. Install

﻿
1) yum install -y https://dev.mysql.com/get/mysql80-community-release-el7-5.noarch.rpm

2) yum repolist enabled | grep "mysql.*"

3) yum install -y mysql-server

4) mysql -V
​

2. Configuring Server

​

1) start & stop

systemctl enable mysqld # 재부팅 시 자동 시작하도록 설정
systemctl start mysqld # 서비스 시작
systemctl status mysqld # 서비스 구동 여부 확인
2) Settings root password 

grep "temporary password" /var/log/mysqld.log
3) root 로 접속 후 비밀 번호 변경(반드시 영문대소숫자특수문자) - Abcdef123! 

mysql -u root -p

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '변경할 비밀번호';
mysql> exit;
4) 비밀 정책 변경

mysql -u root -p

mysql> SHOW VARIABLES LIKE 'validate_password%';
mysql> SET GLOBAL validate_password.length = 5; # 다섯자리까지만..
mysql> SET GLOBAL validate_password.number_count = 0; # 숫자 필요없엉
mysql> SET GLOBAL validate_password.policy=LOW; # 정책 수준은 낮게..
mysql> SET GLOBAL validate_password.mixed_case_count = 0; # 대소문자 필요없엉
mysql> SET GLOBAL validate_password.special_char_count = 0; # 특수문자 필요없엉
5) 사용자 생성 및 데이터베이스 생성

mysql -u root -p

mysql> use mysql;
mysql> select host, user from user;

mysql> CREATE USER '아이디'@'%' identified by '비밀번호'; # 외부접속만 가능한 계정 생성
mysql> CREATE DATABASE DB이름 default character set utf8;
mysql> GRANT ALL PRIVILEGES ON DB이름.* to '아이디'@'%'; # 해당 DB에 대한 권한 부여
mysql> flush privileges; # 새로고침 똭!
mysql> select host,user from user; # 다시 확인
```

### 3.[Centos] PC 파일 전송 압축 해제

```
1. 명령프롬프트 실행 후 

디렉토리를 zip 형태로 묶은 파일을 전송

scp -P 65500 a.zip root@101.101.167.122:/root
2. CentOS 접속 후 압출 해제

unzip  a.zip
```
