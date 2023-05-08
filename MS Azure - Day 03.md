# 2023/05/03 MS Azure Day03

**목  차**

[**1.**](#_1fob9te)[   ](#_1fob9te)**배포 환경** 

[**1.1**](#_3znysh7)[  ](#_3znysh7)**배포 환경** 

[**1.2**](#_2et92p0)[  ](#_2et92p0)**배포 목표** 

[**1.3**](#_tyjcwt)[  ](#_tyjcwt)**Azure** **관련 계정** 

[**2.**](#_3dy6vkm)[   ](#_3dy6vkm)**초기 배포 환경 설정 및 생성**

[**2.1**](#_1t3h5sf)[  ](#_1t3h5sf)**Azure portal** **접속**      

[**2.2**](#_4d34og8)[  ](#_4d34og8)**Azure Database for MySQL** **생성** 

[**2.3**](#_2s8eyo1)[  ](#_2s8eyo1)**App Service** **생성** 

[**3.**](#_3dy6vkm)[   ](#_3dy6vkm)**재배포, 변경사항 적용**

 

 

**1.**  **배포 환경**

**1.1**    **배포 환경**

| 클라우드  플랫폼 | 비고                   |
| ---------------- | ---------------------- |
| Microsoft  Azure | Microsoft Azure Portal |

 

**1.2**    **배포 목표** 

| 제품명                   | 요금제 | 비고                     |
| ------------------------ | ------ | ------------------------ |
| App  service             | F1     | Linux / Java 11 / Tomcat |
| Azure Database for MySQL | B1s    | MySQL 5.7                |

**2.**  **초기 배포 환경 설정 및 생성**

**2.1**    **Azure Portal** **접속**

 



​                                  

![img](C:\Users\min\Documents\TIL\assets\clip_image002.jpg)



\-    리소스 만들기 Tab 클릭

**2.2**    **Azure Database for MySQL** **생성**

\-    Mysql 검색



​                                  

![img](C:\Users\min\Documents\TIL\assets\clip_image004.jpg)



\-    만들기

\-    Azure Database For MySQL

 

 

 

 

 

 

l **유연한 서버 만들기**



​                                  

![img](C:\Users\min\Documents\TIL\assets\clip_image006.gif)



l **기본 설정**



​                                  

![img](C:\Users\min\Documents\TIL\assets\clip_image008.jpg)



\-    리소스 그룹 새로 만들기, 또는 기존 리소스 그룹 선택



​                                  

​                                  

​                                  

![img](C:\Users\min\Documents\TIL\assets\clip_image010.jpg)



\-    서버 이름 입력

\-    지역 Korea Central

\-    MySQL 버전 5.7



​                                  

​                                  

​                                  

​                                  

![img](C:\Users\min\Documents\TIL\assets\clip_image012.jpg)



\-    관리자 사용자 이름 (nshop)

\-    암호 (!ncomz1234) 설정

\-    다음: 네트워킹

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

l **네트워킹**



​                                  

​                                  

​                                  

![img](C:\Users\min\Documents\TIL\assets\clip_image014.jpg)



\-    Azure 내의 모든 Auzre 서비스의 이 서버에 대한 퍼블릭 액세스 허용 체크 (Azure 앱 서비스 배포시 활용)

\-    방화벽 규칙 필요시 적용

\-    다음 : 보안

 

l **보안, 태그, 검토 + 만들기**

\-    생략 후  검토 + 만들기 내용 체크 후 만들기

\-    생성 후 리소스 확인

 

 

 

 

 

 

 

l **연결 탭**

![img](C:\Users\min\Documents\TIL\assets\clip_image016.jpg)

\-    해당 가이드에 맞게 MySQL WorkBench 또는 앱에서 연결

\-    기존 데이터가 있을 시 만들어진 DB로 Dump 실행

**2.3**    **App Service** **생성**

l 

​                                  

 **웹 앱 만들기**



![img](C:\Users\min\Documents\TIL\assets\clip_image018.gif)

 

 

 

 

l **기본**



​                                  

​                                  

​                                  

![img](C:\Users\min\Documents\TIL\assets\clip_image020.gif)





​                                  

​                                  

​                                  

​                                  

​                                  

![img](C:\Users\min\Documents\TIL\assets\clip_image022.jpg)



\-    선택

\-    리소스 그룹 선택

\-    웹 앱 이름 선택 (nshop)

\-    런타임 스택 Java 11 선택

\-    Java 웹 서버 스택 Apache Tomcat 9.0

\-    지역 Korea Central 선택

\-    Linux 플랜 (Korea Central) F1 선택

l **배포, 네트워킹, 모니터링, 태그, 검토 + 만들기**

\-    기본값 적용

\-    모니터링 Application Insights 사용 안함

\-    검토 + 만들기단에서 세부 정보 및 설정 확인 후 만들기

\-    배포 성공 시 리소스 확인

**2.4**    **프로젝트 적용 후 배포**



​                                  

![img](C:\Users\min\Documents\TIL\assets\clip_image024.jpg)



\-    코드 소스 선택 로컬 Git

\-    저장



​                                  

![img](C:\Users\min\Documents\TIL\assets\clip_image026.jpg)



\-    Git Clone URI 해당 주소로 로컬 환경에 Clone 

\-    해당 권한 증명 관련은 로컬Git/FTPS자격 증명탭 참고

\-    로컬 컴퓨터내 클론 받은 Repository에 해당 프로젝트 war파일 복사 후 commit, push

\-    로그 탭 확인

\-    배포 성공

 

 

 

 

 

 

 

 

 

 

 

 

**3.**  **재배포, 변경사항 적용**



​                                  

![img](C:\Users\min\Documents\TIL\assets\clip_image028.jpg)



 

\-    Git Clone URI로 로컬 컴퓨터에 clone, 해당 Repository에 war파일 변경 commit-push



### dataBase Server

- sql_mode 확인 strict** 또는 group by 등 설정 때문에 오류 나는 것 확인





## Daily Issue

1. 공통 - 사용자 그룹 수정 Uncaught ReferenceError: getCurrentPage is not defined
2. 어드민 등록시 상점 정보 등록 - 우편번 찾기 X , 등록시 파일 이미지 등록 X

3. 상품 수정 아안
