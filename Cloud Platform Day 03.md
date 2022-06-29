# 6/29 Cloud Platform Day 03

> 공인 IP : 49.50.162.29 

## [CentOS] Apach-Tomcat 설치

1. JDK1.8 설치 후 진행
2. Apach Tomcat Download

```
wget http://archive.apache.org/dist/tomcat/tomcat-8/v8.5.27/bin/apache-tomcat-8.5.27.tar.gz
```

> tar.gz : zip파일과 같은 압축파일

3. 압축 해제

```
tar zxvf apache-tomcat-8.5.27.tar.gz
```

4. 실행

```
cd /root/apache-tomcat-8.5.27/bin
./startup.sh         -실행
./shutdown.sh      -정지
```

5. 실행 확인

`ps -ef | grep tomcat`

6. PC에서 접속

http://공인아이피:8080/

## 현재 PC에서 [CentOS]로 war파일 전송 후 아파치 톰캣 서버에 띄우기

> http://49.50.162.29:8080/ - webapps에 루트폴더를 나타냄
>
> http://49.50.162.29:8080/day05/ - webapps에 day05폴더를 나타냄

1. 이클립스에서 war파일로 export

2. PC의 DOS(명령 프롬프트)에서 `scp -P 60003 day05.war root@49.236.136.104:/root`

   로 Centos로 보내기

3. 보내진 war파일 Putty에서 `/root/apache-tomcat-5.5.27/webapps` 로 복사

4. `/root/apache-tomcat-5.5.27/bin`에서 `.startup.sh`로 실행 후 확인

5. `http://49.50.162.29:8080/day05/`와 같은 형식으로 서버 호출

6. `/root/apache-tomcat-5.5.27/webapps` 에서 ROOT를 ROOT_BACK 으로 이름 변경 후 day05를 ROOT로 변경

   - 8080을 쳤을때 바로 들어가기 위함

   - 이름 변경 방법

     - mv 변경할내용 변경할이름

       mv ROOT ROOT_BACK

       mv day05 ROOT

### 1) [CentOS] 복사하는 방법

`cp /root/day05.war .`

- cp : 복사 코드
- /root/day05.war : 복사할 대상
- . : 복사할 위치 (현재위치를 의미)

