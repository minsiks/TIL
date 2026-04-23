# Jenkins CI/CD 구성 가이드
> SVN → Maven Build → SFTP 배포 → 이중 서버 Rolling 배포 구성 정리

---

## 목차
1. [환경 개요](#1-환경-개요)
2. [Jenkins 설치 (로컬)](#2-jenkins-설치-로컬)
3. [필수 플러그인 설치](#3-필수-플러그인-설치)
4. [Jenkins Job 구성 순서](#4-jenkins-job-구성-순서)
5. [Step별 상세 설정](#5-step별-상세-설정)
   - [Step 1. SCM - SVN 연동](#step-1-scm---svn-연동)
   - [Step 2. 빌드 전 처리 - Properties 파일 교체](#step-2-빌드-전-처리---properties-파일-교체)
   - [Step 3. 불필요 라이브러리 삭제](#step-3-불필요-라이브러리-삭제)
   - [Step 4. Maven Build (clean package)](#step-4-maven-build-clean-package)
   - [Step 5. SFTP 업로드](#step-5-sftp-업로드)
   - [Step 6. Remote Command - Server 1 배포](#step-6-remote-command---server-1-배포)
   - [Step 7. Remote Command - Server 2 배포](#step-7-remote-command---server-2-배포)
6. [전체 파이프라인 흐름 요약](#6-전체-파이프라인-흐름-요약)
7. [추가로 알아두면 좋은 것들](#7-추가로-알아두면-좋은-것들)

---

## 1. 환경 개요

| 항목 | 내용 |
|------|------|
| Jenkins 위치 | 로컬 PC |
| 형상관리 | SVN |
| 빌드 도구 | Maven |
| 배포 산출물 | `.war` 파일 |
| 배포 방식 | SFTP (SSH) |
| 타겟 서버 | 원격 서버 2대 (Rolling 방식) |

---

## 2. Jenkins 설치 (로컬)

### Windows 기준

```bash
# Jenkins 공식 다운로드
https://www.jenkins.io/download/

# 또는 WAR 파일 직접 실행
java -jar jenkins.war --httpPort=8080
```

### 초기 설정

```
1. 브라우저에서 http://localhost:8080 접속
2. /var/jenkins_home/secrets/initialAdminPassword (또는 화면에 표시된 경로) 에서 초기 비밀번호 확인
3. 추천 플러그인 설치 선택
4. 관리자 계정 생성
```

---

## 3. 필수 플러그인 설치

> Jenkins 관리 → 플러그인 관리에서 설치

| 플러그인 | 용도 |
|----------|------|
| **Subversion Plugin** | SVN 체크아웃 연동 |
| **Maven Integration Plugin** | Maven 빌드 실행 |
| **Publish Over SSH** | SFTP 파일 전송 + 원격 명령 실행 |
| **SSH Plugin** | 원격 서버 SSH 명령 실행 (보조) |

---

## 4. Jenkins Job 구성 순서

```
[SVN Checkout]
      ↓
[Properties 파일 교체 (.prod → 운영용으로 덮어쓰기)]
      ↓
[불필요 라이브러리 삭제]
      ↓
[Maven clean package]
      ↓
[SFTP로 WAR 업로드]
      ↓
[Server 1: Stop → Deploy Shell → Start]
      ↓
[Server 2: Stop → Start]
```

---

## 5. Step별 상세 설정

### Step 1. SCM - SVN 연동

**Job 설정 → 소스 코드 관리 → Subversion**

```
Repository URL  : http://svn.myserver.com/repos/myproject/trunk
Credentials     : SVN 계정 (Username/Password 방식으로 등록)
Local module    : . (루트에 체크아웃)
Check-out Strategy : Use 'svn update' as much as possible (빠름)
```

> **Credentials 등록 위치**: Jenkins 관리 → Credentials → System → Global credentials

---

### Step 2. 빌드 전 처리 - Properties 파일 교체

**Build Steps → Execute shell (Linux) 또는 Execute Windows batch command**

#### 목적
- SVN에는 로컬 개발용 properties가 존재
- 운영 서버용 설정은 별도 `.prod` 파일로 관리
- 빌드 전 `.prod` 파일을 원본 파일에 덮어써서 운영 설정으로 교체

#### 예시 Shell

```bash
#!/bin/bash

PROP_DIR="src/main/resources"

echo "=== Properties 파일 교체 시작 ==="

# .prod 파일을 원본으로 복사 (덮어쓰기)
cp ${PROP_DIR}/application.properties.prod ${PROP_DIR}/application.properties
cp ${PROP_DIR}/db.properties.prod          ${PROP_DIR}/db.properties

echo "=== .prod 원본 파일 삭제 ==="

# 교체 후 .prod 파일 제거 (WAR에 포함되지 않도록)
rm -f ${PROP_DIR}/*.prod

echo "=== Properties 교체 완료 ==="
```

> **.prod 파일 관리 팁**: `.prod` 파일은 SVN에 포함시키되 `svn:ignore`로 IDE 추적 제외하거나,  
> 별도 보안 저장소(Vault, Jenkins Credentials)로 관리하는 것을 권장

---

### Step 3. 불필요 라이브러리 삭제

**Build Steps → Execute shell**

```bash
#!/bin/bash

LIB_DIR="src/main/webapp/WEB-INF/lib"

echo "=== 불필요 라이브러리 삭제 시작 ==="

# 예시: 개발 전용 라이브러리 제거
rm -f ${LIB_DIR}/h2-*.jar          # 개발용 인메모리 DB
rm -f ${LIB_DIR}/lombok-*.jar       # 컴파일 후 불필요
rm -f ${LIB_DIR}/devtools-*.jar     # 개발 도구

echo "=== 삭제 완료 ==="
ls ${LIB_DIR}
```

> **대안**: `pom.xml`에서 해당 의존성에 `<scope>provided</scope>` 또는 `<scope>test</scope>` 를 설정하면  
> 빌드 시 자동으로 WAR에서 제외되어 쉘 삭제 과정이 불필요해짐 (권장)

---

### Step 4. Maven Build (clean package)

**Build Steps → Invoke top-level Maven targets**

```
Maven Version : (Jenkins에 등록된 Maven, ex: Maven 3.8.6)
Goals         : clean package -Dmaven.test.skip=true
POM           : pom.xml
```

#### 또는 Execute shell로 직접 실행

```bash
mvn clean package -Dmaven.test.skip=true
```

| 옵션 | 설명 |
|------|------|
| `clean` | 이전 빌드 산출물(target/) 삭제 |
| `package` | 컴파일 + 테스트 + WAR 패키징 |
| `-Dmaven.test.skip=true` | 테스트 생략 (배포 속도 향상) |

> **Maven 등록 위치**: Jenkins 관리 → Global Tool Configuration → Maven

---

### Step 5. SFTP 업로드

**플러그인**: `Publish Over SSH`

**Jenkins 관리 → 시스템 설정 → Publish over SSH** 에서 서버 등록

```
Name        : prod-server          (식별용 이름)
Hostname    : 192.168.1.100        (타겟 서버 IP)
Username    : deploy               (SSH 계정)
Remote Directory : /home/deploy    (기본 원격 경로)
# 인증 방식: Key 방식 권장 (Passphrase + Private Key 입력)
```

**Job 설정 → Post-build Actions → Send build artifacts over SSH**

```
Name            : prod-server
Source files    : target/*.war
Remove prefix   : target/
Remote directory: /opt/deploy/uploads/     (타겟 서버의 업로드 경로)
```

---

### Step 6. Remote Command - Server 1 배포

**위와 동일한 Publish Over SSH 설정 → Exec command**

```bash
# Server 1 배포 순서

echo "[Server1] 서버 중지"
/etc/init.d/tomcat stop
# 또는
sh /home/deploy/scripts/stop.sh

echo "[Server1] 배포 실행"
sh /home/deploy/scripts/deploy.sh

echo "[Server1] 서버 시작"
sh /home/deploy/scripts/start.sh

echo "[Server1] 배포 완료"
```

#### deploy.sh 예시 (타겟 서버에 존재하는 쉘)

```bash
#!/bin/bash
APP_DIR="/opt/tomcat/webapps"
UPLOAD_DIR="/opt/deploy/uploads"
APP_NAME="myapp"

# 기존 배포 파일 백업
mv ${APP_DIR}/${APP_NAME}.war ${APP_DIR}/${APP_NAME}.war.bak 2>/dev/null

# 새 WAR 복사
cp ${UPLOAD_DIR}/${APP_NAME}.war ${APP_DIR}/${APP_NAME}.war

echo "배포 완료: $(date)"
```

---

### Step 7. Remote Command - Server 2 배포

**Server 1 완료 후 별도 SSH Publisher 블록으로 Server 2 처리**

```bash
# Server 2 배포 순서 (Server 1 완료 후 실행)

echo "[Server2] 서버 중지"
sh /home/deploy/scripts/stop.sh

echo "[Server2] 서버 시작"
sh /home/deploy/scripts/start.sh

echo "[Server2] 배포 완료"
```

> Server 1과 Server 2가 **순차 실행**되도록 같은 Job 내 Publish Over SSH를 두 블록으로 구성하거나,  
> **Downstream Job**으로 분리하여 Server 1 성공 후에만 Server 2가 실행되도록 구성 가능

---

## 6. 전체 파이프라인 흐름 요약

```
┌─────────────────────────────────────────────────────┐
│                   Jenkins Job                        │
│                                                     │
│  1. SVN Checkout                                    │
│     └─ trunk 최신 소스 받아오기                      │
│                                                     │
│  2. Execute Shell: Properties 교체                  │
│     └─ *.prod → 원본 덮어쓰기 → .prod 삭제          │
│                                                     │
│  3. Execute Shell: 불필요 라이브러리 삭제            │
│                                                     │
│  4. Maven: clean package                            │
│     └─ target/myapp.war 생성                        │
│                                                     │
│  5. Publish Over SSH: SFTP Upload                   │
│     └─ WAR → 원격 서버 /opt/deploy/uploads/         │
│                                                     │
│  6. SSH Remote Command: Server 1                    │
│     └─ stop.sh → deploy.sh → start.sh              │
│                                                     │
│  7. SSH Remote Command: Server 2                    │
│     └─ stop.sh → start.sh                          │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## 7. 추가로 알아두면 좋은 것들

### 🔒 보안 관련

#### Credentials 관리
```
- SVN 계정, SSH Key, 서버 비밀번호는 절대 Job 설정에 하드코딩 금지
- Jenkins 관리 → Credentials에서 등록 후 참조 방식 사용
- SSH 접속은 비밀번호 방식보다 Key 기반 인증 권장
```

#### Properties 파일 보안
```
- .prod 파일을 SVN에 올리는 것은 보안상 취약
- 대안 1: Jenkins Credentials + 빌드 시 주입 (envInject 플러그인)
- 대안 2: Vault(HashiCorp), AWS Secrets Manager 등 시크릿 관리 도구 연동
```

---

### 📦 배포 안정성

#### 롤백 전략
```bash
# deploy.sh에 롤백 기능 추가 예시
cp ${APP_DIR}/${APP_NAME}.war ${APP_DIR}/${APP_NAME}.war.$(date +%Y%m%d%H%M%S).bak
# 문제 발생 시
cp ${APP_DIR}/${APP_NAME}.war.bak ${APP_DIR}/${APP_NAME}.war
```

#### 헬스체크 추가
```bash
# start.sh 이후 서버 기동 확인
sleep 30
HTTP_STATUS=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:8080/myapp/health)
if [ "$HTTP_STATUS" != "200" ]; then
  echo "서버 기동 실패! 상태코드: $HTTP_STATUS"
  exit 1
fi
echo "헬스체크 통과"
```

#### Zero Downtime 배포 (현재 구성의 한계)
```
현재 구성: Server 1 다운 → 배포 → Server 2 다운 → 배포
→ Server 1 다운 시간 동안 Server 2만으로 서비스 운영되는 구조

개선 방향:
- L4/L7 로드밸런서가 있다면 Server 1을 LB에서 제외 후 배포 → 복귀 후 Server 2 배포
- Blue/Green 배포 전략 도입 고려
```

---

### 🔄 Jenkins Pipeline (Declarative)으로 전환

현재 Freestyle Job 방식에서 **Jenkinsfile** 방식으로 전환하면 코드로 파이프라인을 관리할 수 있음

```groovy
pipeline {
    agent any

    tools {
        maven 'Maven 3.8.6'
    }

    stages {
        stage('Checkout') {
            steps {
                svn url: 'http://svn.myserver.com/repos/myproject/trunk',
                    credentials: 'svn-credentials-id'
            }
        }

        stage('Replace Properties') {
            steps {
                sh '''
                    cp src/main/resources/application.properties.prod \
                       src/main/resources/application.properties
                    rm -f src/main/resources/*.prod
                '''
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package -Dmaven.test.skip=true'
            }
        }

        stage('Deploy to Server1') {
            steps {
                sshPublisher(publishers: [
                    sshPublisherDesc(
                        configName: 'prod-server1',
                        transfers: [sshTransfer(
                            sourceFiles: 'target/*.war',
                            removePrefix: 'target/',
                            remoteDirectory: '/opt/deploy/uploads/',
                            execCommand: 'sh /home/deploy/scripts/deploy-server1.sh'
                        )]
                    )
                ])
            }
        }

        stage('Deploy to Server2') {
            steps {
                sshPublisher(publishers: [
                    sshPublisherDesc(
                        configName: 'prod-server2',
                        transfers: [sshTransfer(
                            execCommand: 'sh /home/deploy/scripts/deploy-server2.sh'
                        )]
                    )
                ])
            }
        }
    }

    post {
        success {
            echo '✅ 배포 성공'
        }
        failure {
            echo '❌ 배포 실패 - 담당자 확인 필요'
        }
    }
}
```

---

### 📬 빌드 알림

#### 이메일 알림
```
Post-build Actions → E-mail Notification
→ 빌드 실패 시 담당자 이메일 자동 발송
```

#### Slack 연동
```
플러그인: Slack Notification Plugin
→ 빌드 성공/실패 시 Slack 채널에 메시지 발송
```

---

### 📊 빌드 이력 관리

```
- Jenkins Job 설정 → 오래된 빌드 삭제 정책 설정 권장
  예: 최근 10개 빌드만 보관, 30일 이상 된 빌드 자동 삭제
- Maven 빌드 캐시 활용: ~/.m2/repository 를 Jenkins 노드에서 공유하면 빌드 속도 향상
```

---

### 🧩 다음 단계로 발전시킬 수 있는 것들

| 주제 | 설명 |
|------|------|
| **Jenkinsfile (Pipeline as Code)** | Job 설정을 코드로 관리, SVN/Git에 함께 버전 관리 |
| **Blue/Green 배포** | 무중단 배포 전략 |
| **Jenkins Shared Library** | 여러 Job에서 공통 파이프라인 코드 재사용 |
| **Docker 기반 빌드** | 빌드 환경을 컨테이너로 격리 |
| **SonarQube 연동** | 정적 코드 분석 자동화 |
| **Maven Profile** | 환경별 Properties를 Maven Profile로 관리 (`-P prod`) |
| **Artifact Repository** | Nexus/Artifactory로 WAR 아티팩트 버전 관리 |

---

*작성일: 2026년 4월 | 구성 환경: Jenkins (Local) + SVN + Maven + SSH*