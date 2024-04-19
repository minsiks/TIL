# 24-04-18 AgensSQL Day 05

## AgensSQL



### 1.AgensSQL Command Line Interface



- O/S Level Command

  - agensSQL Version 조회

    - ```linux
      ag_ctl --version
      ```

      

  - AgensSQL 상태확인

    - ```
      systemctl status | grep ags
      systemctl status ags-14
      ```

      

  - AgensSQL 기동/종료/상태 정보 조회

    - ```
      ag_ctl (start|stop|restart|status) [-D $PGDATA]
      ag_ctl status
      ```

      

  - Database 접속

    - asql -U agens -d agensdb -p 5333

- Database Level Command - Operation Command

  - Database 목록조회

    - ```postgresql
      // select * from pg_catalog.pg_database 조회결과와 유사
      \l
      //조회 확장
      \l+
      // select current_database();
      agensdb=# \conninfo
      접속정보: 데이터베이스="agensdb", 사용자="agens", 소켓="/var/run/ags", 포트="5333".
      
      // prostgres DB로 접속 변경
      agensdb=# \c postgres
      접속정보: 데이터베이스="postgres", 사용자="agens".
      
      postgres=# \conninfo
      접속정보: 데이터베이스="postgres", 사용자="agens", 소켓="/var/run/ags", 포트="5333".
      ```

      

  - 현재 Database 이름 확인

    - ```
      // select * from pg_catalog.pg_tablespace;
      agensdb=# \db+
      ```

      

  - Tablespace 목록조회, 사용자 목록 조회

    - ```
      agensdb=# select * from pg_catalog.pg_user;
      // \dg 명령어도 동일
      agensdb=# \du
      ```

      

  - Role(사용자 권한) 정보 조회

    - ```
      // \dg 명령어도 동일
      agensdb=# \du
                                      롤 목록
       롤 이름 |                      속성                      | 소속 그룹:
      ---------+------------------------------------------------+------------
       agens   | 슈퍼유저, 롤 만들기, DB 만들기, 복제, RLS 통과 | {}
       test    |                                                | {}
      ```

      

  - Table, View, Sequence 목록 조회

    - ```
      agensdb=# \d
      ```

      

  - 이전 (bufferd) Query 재실행

    - ```
      \g
      ```

      

- Database Level Command - Display Opions

  - ```
    \x 칼럼 단위 보기 기능 켬
    Column을 Row로 변경 출력
    ```

    

  - ```
    \a 
    Column 길이 무시하고 출력
    ```

    

  - ```
    \t 자료만 보기 기능 켬
    Column 명(Tuple header) 제외하고 출력
    ```

    

  - ```
    \timing 작업 수행시간 보임
    Query 실행 시간 출력
    ```

    

### 2. vi 편집기 사용



- 이동

  - ```
    h : 왼쪽이동
    j : 아래로 이동
    k : 위로 커서이동
    l : 오른쪽 이동
    ^ : 행의 맨 왼쪽으로 커서 이동
    $ : 행의 맨 오른쪽으로 커서 이동
    ```

    

- 변경, 삭제, 복제

  - ```linux
    d + {} : 기본적인 delte 수행
    dw : 현재 커서가 있는 한 단어 삭제 
    dd : 한줄 삭제
    Y : 행 yank 또는 복사 
    yy : 행 yank 또는 복사
    y + {} : 복사 수행 시작
    yw : 현재 커서에 있는 한 단어 복사
    c + {} : 변경 수행 시작
    cw : 현재 커서가 있는 한 단어 변경
    p : 붙여넣기
    ```

    

### 4.17 오전 Agens 교육

![image-20240419151022034](C:\Users\min\Documents\TIL\assets\image-20240419151022034.png)

- 설치 및 환경설정
  - 바이너리, rpm 두가지 존재
  - agensSQL : PostgreSQL + Oracle 호환성
  - EDB
  - oltp olap
    - oltp (Online Transaction Processing)
      - '운영'계 데이터 및 데이터를 처리하는 방법을 의미, 복수의 사용자 PC에서 발생되는 트랜잭션을 DB 서버가 처리
      - 비교적 작은 규모의 트랜잭션들로 구성
    - olap (Online Analytical Processing)
      - '분석'계 데이터 및 데이터를 처리하는 방법을 의미
      - 데이터 웨어하우스 (DW), DB에 저장되어 있는 데이터를 부넉
      - 대용량데이터를 처리
  - SHared_buffer, 워크 메모리,io
    - io : Input/Output
    - Shared buffer : 공유 리소스, 캐시 등등을 저
    - 워크 메모리 : sort, merge, join 등에 사용하는 메모리 사용
  - db 메모리설정
  - pg_hba.conf
    - 접근 허용등 중요한 환경설정

### 3. PostgreSQL Architecture



- PostgreSQL란
  - 객체 관계형 데이터 베이스(ORDBMS)
  - 데이터 베이스 랭킹 4위
  - 오픈소스로 제공되며 다양한 파생 모델들도 존재



- Memory 영역

[![image-20240418150952053]()](https://github.com/minsiks/TIL/blob/master/AgensSQL - Day 04.md)

모든 프로세서가 공유해서 사용하는 Shared Memory

PostgreSQL의 Backend 프로세스 별로 할당되는 Local Memory

- Shared Memory

  - Shared buffers
    - 디스크에서 필요한 데이터를 shared buffer 에서 미리 처리, disk i/o를 줄여 성능 확보
    - 서버 메모리의 4분의 1 권장, postgresql.conf 에서 설정 가능(default=128MB)
    - buffer manger의 활용 빈도가 page(=디스크 상의 block)를 캐싱하고, clock sweep 알고리즘을 통해 효율적인 관리
  - WAL(write Ahead Logging) Buuffer
    - 데이터의 변경사항을 WAL file로 만들어 디스크 영역으로 내리기 전까지 보관
    - 복구 작업 시 data의 재구성을 위해 필요
    - wal_buffers 파라미터 값으로 조정
  - CLog buffer (Commit Log Buffer)
    - 트랜잭션의 상태를 캐싱하는 메모리 공간으로 어떤 트랜잭션이 commit 되었는지 확인
    - 각 트랜잭션 (in_progress, commited, aborted, sub_commited)의 상태를 기록하여 완료 여부 확인
    - DB에 의해 자동으로 관리
  - Lock space
    - 동시성 제어에서 동시에 어떤 항목에 접근을 시도, 충돌 방지, 허용에 대한 기능
    - 모든 유형의 Lock 정보를 저장

- Local Memory (Process Memory)

  개별 프로세스가 할당 받아 사용하는 메모리 공간, 세션 단위로 할당되지만 트랜잭션 단위로도 가능

  - Maintenance work memory
    - 유지 관리 작업에 사용되는 메모리
    - Vaccum 관련 작업, 인덱스 생성, 테이블 변경, FK 추가 등의 작업에 사용
  - Temp buffer
    - Temp 테이블을 사용하는 경우에만 할당
  - Work memory
    - Sort/Hash 등의 연산작업으로 Temp 파일을 사용하기 전에 사용
  - Catalog cache
    - 시스템 catalog 메타데이터를 이용할때 사용
  - Optimizer /Executer
    - 실행된 query의 실행 계획 및 실행을 담당

- PostgreSQL Processes

  메인 프로세스

  - Postmaster
    - 메인 프로세스, 가장 먼저 시작, Shared memory영역 할당
    - 하위 프로세스들의 작동 유무 체크 및 문제 발생 시 재기동
    - Client 연결 요청 대기, 요청 시 Postgres 프로세스 생성
  - Postgres
    - Client 가 요청한 SQL 및 Command의 처리 프로세스, Client 연결 해제 시 종료
    - Client:Progres 1:1 관계, 개수는 max_connections 파라미터로 조정(default=100)

  백그라운드 프로세스

  - BG writer
    - Shared buffer 내에 dirty page를 디스크로 주기적인 flush 수행, checkpoint 시 필연적으로 발생하는 I/O의 분산
    - check point 발생 시 flush 수행
  - WAL writer
    - WAL buffer를 주기적으로 확인
    - 트랜잭션의 커밋이나 로그 파일 공간이 찼을 경우 실행
  - checkpointer
    - checkpoint를 수행하며, 이전의 모든 변경 사항을 disk로 flush
    - 다운이나 충돌 시 마지막 checkpoint 레코드를 확인하여 복구
  - Archiver
    - 자동으로 최신화 되어 삭제되는 구 버전의 WAL 파일의 archiving을 진행하는 프로세스
  - Stats Collector
    - DB의 통계정보를 수집
    - Session 정보(pg_stat_activity) 및 테이블 통계정보와 같은 DBMS 사용 통계를 수집하여 pg_catalog에 정보를 업데이트
    - Optimizer가 최적의 query 실행을 위해 해당 정보 참조
  - Autovacuum launcher
    - vaccum이 필요한 시점에 autovaccum을 수행하고 관리하는 프로세스
  - Logger
    - 오류나 프로세스 정보를 기록하며 로그 메시지로 저장하는 프로세스
    - $PGDATA/pg_log 폴더에 저

> flush : 데이터를 메모리나 캐시에서 디스크로 영구적으로 기록하는 작업을 의

```

```

