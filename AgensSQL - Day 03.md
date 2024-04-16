# 24-04-16 AgensSQL Day 03

## AgensSQL 

### 1. 리눅스에서 AgensSQL 설치

> 리눅스 명령어
>
> sed : 파일에서 조건에 맞는 내용을 추출하는 명령
>
> mv : 파일 옮기기
>
> **tar -zxvf [파일명.tar.gz]** : tar.gz파일 압축 풀기
>
> chown : 명령어는 파일 및 디렉토리의 소유권을 바꾸는 명령어파일 옮기기 편리
>
> cat : 리눅스 파일 출력
>
> grep : 특정 파일에서 지정한 문자열이나 정규표현식을 포함하는 행을 출력해주는 명령어
>
> systemctl (명령)(서비스명) : 서비스 제어 명령어
>
> - start, stop, status, restart, reload 등
>
> echo : 출력을 담당하는 명령어
>
> source : 스크립트 파일을 수정한 후 수정된 값을 바로 적용하기 위해 사용하는 명령어
>
> du : 디렉토리의 용량 계산 `du -sh`

- 리눅스 ssh 연결 (root계정 login 허용방법)

  - systemctl status ssh 입력 시 현재상태 확인
  - active (running) 상태가 아니라면 systemctl enable ssh 실행
  - vi /etc/ssh/sshd_config 
    - sshd 설정 파일을 편집
    - :/PermitRootLogin 검색
    - PermitRootLogin yes 주석 해제

  - systemctl restart sshd

####  * sed (Stream Editor) 명령어의 사용방법

- 편집기와 비슷
- 문자열을 치환하는 등 다양한 작업을 수행 가능
- 파일에서 조건에 맞는 내용으로 추출하는데 중점
- 자주 사용하는 sed 명령어 사용 예시
  - sed '[패턴]' [파일명]
  - cat [파일명] | sed '[패턴]'
  - cat 명령으로 파일을 확인한 뒤 파이프라인을 이용해 패턴을 적용 할 수 도 있다.
- -i : 'in-place' 편집을 나타냄 / 원본 파일을 직접 수정

#### *  쉘 스크립트

- Unix커맨드등을 나열해서 실행
- 기본적으로 .sh 확장자로 작성
- #로 코멘트 처리

### 2. Agens SQL 시작하기

- Database 기동

  - AgensSQL 서비스 관리는 ag_ctl 명령어를 이용하여 관리

  - 아래와 같은 ag_ctl 명령어를 통해 AgensSQL 데이터 베이스 서버를 초기화, 시작, 중지, 재시작, 서버 상태를 출력하는 등의 작업을 수행

    - ```ss
      ag_ctl(start | stop | restart | status) [-D DATADIR]
      ```

    - -D 옵션 추가시 PGDATA 경로를 지정 (미설정시 .bash profile에 지정된 PGDATA를 기본으로 동작)

- DB 접속

  - asql 명령어를 이용하여 접속 가능

  - ```linux
    asql [-h | -p | -U | id]
    ```

    - -h: 접속 IP (기본값 Localhost)
    - -p : 접속 Port (기본값 5432)
    - -U : 접속 User (기본값 OS User)
    - -d : 접속 Database (기본값 OS User명과 동일)

- AgensUser Password 설정

  - pg_hba.conf에서 메소드가 peer이기에 별도의 패스워드 없이 바로 접속 가능

    - pg_hba.conf 경로 : /var/lib/agens/ags-14/data/pg_hba.conf

  - 패스워드 설정 및 변경

    ```sql
    Alter user agens with password 'agens';
    ```

  - pg_hba.conf 파일의 접속 method를 패스워드 방식 (scram-sha-256)으로 변경

- DB중지

  - ```linux
    ag_ctl stop -m[smart | fast | immediate]
    ```

    - -m : 옵션을 통해 중지 방법을 선택
    - smart : 모든 클라이언트의 연결이 끊기게 되면 중지
    - fast : 클라이언트의 연결을 강제로 끊고 정상적으로 중지
    - immediate : 무조건 중지

  - DB 중지할 시 주의해야 할 점

    - rpm으로 설치 시 agens유저에서 중지 하면 안되고 root 유저로 변환하여 중지해야함

### 3. User 관리

Roles는 Users와 Groups를 모두 포괄하는 개념

처음 AgensSQL을 설치하면 기본으로 agens user가 생성

- User 접속

  - initdb 수행 시  지정했던 superuser를 통해 접속이 가능

- User 생성

  - ```sql
    CREATE USER test_user;
    ```

- 권한 부여

  - ```sql
    ALTER USER test_user WITH [ Superuser | createrole | createDB | Replication | BypassRLS]
    ```

- User 삭제

  - ```sql
    DROP USER test user;
    ```

### 4. AgensSQL Physicall 영역 File 확인

% schema : Object의 논리적 집합

% database : 여러 Schema로 구성

% PostgreSQL 서버 : 여러 Database를 관리

% Cluster : 하나의 서버가 관리

 ![2024-04-16 11 22 08](C:\Users\min\Documents\TIL\assets\2024-04-16 11 22 08.png)

% Database Cluster의 요소들은 일반적으로 data 경로 디렉토리 하위에 위치

- Data 영역

  - global 디렉토리
    - pg_database와 같은 클러스터 전체 테이블을 포함하는 하위 디렉토리
  - base 디렉토리
    - 데이터베이스 별 하위 디렉토리를 포함하는 하위 디렉토리
    - Database내에 있는 테이블, 인덱스, 함수와 같은 Object가 실제로 저장되는 공간
    - 저장경로 : $PGDATA/base/{database_OID}/{object_id} 순으로 구성

  - pg_tblspc 디렉토리
    - AgensSQL의 Tablespace 정보가저장되는 디렉토리
    - Data 디렉토리가 아닌 다른 특정 디렉토리를 지정하여 Tablespace를 생성하면, 해당 디렉토리의 경로가 pg_tblspc 디렉토리에 생성된 심볼릭 링크를 통해 연결

- Log 영역

  - pg_wal Directory
    - 비정상 종료시 복구 용도로 사용되는 WAL파일을 저장
  - Log Directory
    - 특이사항들을 기록하는 디렉토리

- Configuration 영역

  - postgresql.conf
    - PostgreSQL Database의 기본 환경설정 파일
  - pg_hba.conf
    - PostgreSQL의 인증시스템 관련 정보를 담고 있다
    - PostgreSQL에 접근하는 Host나 Host의 Data 전송방식, 암호화 전송방식에 대한 설정 가능
    - 변경 후 PostgreSQL 재기동 필수
  - postmaster.pid
    - 현재 포스트 마스터 프로세스 ID(PID)
    - 클러스터 데이터 디렉토리 경로 등 기록하는 파일
    - 이 파일은 서버 종료 후에 존재 X
  - pg_ident.conf
    - 클라이언트의 인증 방식으로 Ident 인증을 사용하는 경우, ident사용자 이름을 PostgreSQL의 역할 이름에 매핑 사용
  - postgresql.auto.conf
    - ALTER SYSTEM에서 설정한 구성 매개 변수를 저장하는 데 사용되는 파일

### AgensSQL 주요 File Size 관리

DB관리자는 File system 사용량 확인을 주기적으로 진행하여 Data file Systme이 Full 상태일 때 발생할 수 있는 오류를 사전에 방지

- File system 사용량 확인

  - ```linux
    df -h
    ```

- data Files, Wal Files, Log Files, Archive Files

  - ```linux
    #Data Files 확인
    $ cd $PGDATA/base
    $ du -sh *
    
    #Wal Files 확인
    $ cd $PGDATA/pg_wal
    $ du -sh *
    
    #Log Files 확인
    $ cd $PGDATA/log
    $ du -sh *
    * postgresgl.conf 파일의 logging_collector의 설정값이 on 일경우 $PGDATA/log 디렉토리가 생성됨
    #Archive Files 확인
    $ cd SPGDATA/{사용자가 설정한 Archive 경로} 
    $ du -sh *
    ```

- 전체 데이터베이스 사이즈 조회

  - ```sql
    SELECT pg_database_size('agensdb');
    ```

- 개별 테이블 사이즈 조회

  - ```postgresql
    SELECT pg_relation_size('app.object','fsm');
    SELECT pg_indexes_size ('object_property_pkey');
    SELECT pg_size_pretty (pg_total_relation_size('tmp_user'));
    ```

- 임시파일

  - 정렬 및 해시 작업을 수행하는 쿼리는 인스턴스 메모리를 사용하여 work_mem 파라미터에 지정된 값까지 결과를 저장

  - 인스턴스 메모리가 충분하지 않은 경우 임시파일이 생성 기록

  - 쿼리가 완료된 후에는 자동을 제거

  - ```postgresql
    SELECT datname,temp_files AS "Temporary files", temp_bytes AS "Size of temporary files" FROM pg_stat_database;
    ```

- 용량 FULL일 경우 조치

  - File system과 Log File을 조회하여 불필요한 파일을 확인하여 삭제하거나, 이동하는 방법으로 용량을 관리

### 6. Tablespaces 관리

% Tablespace를 사용하여 DB관리자는 오브젝트를 나타내는 파일을 저장할 수 잇는 file system의 위치를 정의

% 데이터베이스의 목적에 따라 저장소를 다르게 사용하는 유연한 운영이 가능

% 주의할 점 : Tablespace도 Database Cluster의 필수적인 부분이기 때문에 파티션 위치가 다르더라도 백업이나 이관시 함께 수행, HA 구성 시에도 양쪽 서버에 동일한 디렉토리를 생성 후 작업

- Tablespace 추가 생성

  - ```sql
    CREATE TABLESPACE tablespace_name
      [ OWNER { new_owner | CURRENT_USER | SESSION_USER } ]
      LOCATION ‘directory’
    [ WITH ( tablespace_option = value [ , … ] ) ]
    ```

### 7. Vacuum 수행 관리

% DB 및 테이블 팽창문제, 불필요한 인덱스 증가문제, I/O 증가 문제를 PostgreSQL의 Vacuum을 통해 관리 가능

- Vacuum 성능 최적화

  - autovacuum_vacuum_scale_factor를 0으로 설정

    - 아래와 같은 쿼리 설정을 통해 테이블에 dead tuple이 100,000개가 생성될 때 마다 Autovacuum이 동작하게 됩니다.

    - ```sql
      ALTER TABLE sample_table SET (autovacuum_vacuum_scale_factor = 0.0);
      ALTER TABLE sample_table SET (autovacuum_vacuum_threshold = 100000);
      ```

  - autovacuum_vacuum_cost_limit을 증가

    - autovacuum_vacuum_cost_limit 설정은 Autovacuum이 한 번 동작할 때의 동작 시간을 결정하게 됩니다. vacuum_cost와 관련된 기본 설정은 아래와 같습니다

    - ```sql
      vacuum_cost_delay = 0
      vacuum_cost_page_hit = 1
      vacuum_cost_page_miss = 10
      vacuum_cost_page_dirty = 20
      vacuum_cost_limit = 200
      
      autovacuum_vacuum_cost_limit = -1
      ```

    - l  autovacuum_vacuum_cost_limit이 -1일 경우, 해당 값은 vacuum_cost_limit을 참조합니다.

      l  Autovacuum이 한 번 실행될 때, 해당 프로세스는 200의 credit을 소유합니다.

      l  page_hit 영역(shared buffer 영역)에 있는 데이터를 vacuuming 할 때 마다 1의 credit을 소모합니다.

      l  page_miss 영역(디스크 영역)에 있는 데이터를 vacumming 할 때 마다 10의 credit을 소모합니다.

      l  page_dirty 영역에 있는 데이터를 vacuuming 할 때 마다 20의 credit을 소모합니다.

      l  200의 credit이 모두 소진되면 해당 Autovacuum 프로세스는 종료됩니다.

    - autovacuum_analyze_scale_factor 테이블 별 설정

      - AgensSQL은 Autovacuum 데몬을 통해 주기적으로 분석 데이터를 수집하여 해당 테이블에 select 쿼리를 수행할 때의 최적의 실행 계획을 수립하게 됩니다. 아래 쿼리를 통해 dead tuple을 최소화할 수 있습니다.

- Maximum XID 관리

  - AgensSQL 에서 하나의 tuple은 두개의 트랜잭션 ID(XID)
    - xmin : 튜플이 insert 될 때 이후 트랜잭션이 볼 수 있도록 가지는 id
    - xmax : Update, Delete가 될 때 해당 시점 Id

- Dead Tuple 관리

  - 모니터링을 통해 테이블에 있는 dead tuple/live tuple의 비율과 Vacuum의 최근 수행 이력 및 dead tuple 식별용 샘플을 확인 가능

> . systemctl 등록
> 4.1 ags-14.service 파일 업로드 및 실행
> - 경로 : /usr/lib/systemd/system/
>
> ```
> # systemctl daemon-reload 
> 
> # systemctl start ags-14.service
> 
> # systemctl enable ags-14.service
> 
> # systemctl status ags-14.service
> ```

