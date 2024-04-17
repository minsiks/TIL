# 24-04-17 AgensSQL Day 04

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

    - ```sql
      agensdb=# select * from pg_catalog.pg_user;
      // \dg 명령어도 동일
      agensdb=# \du
      ```

  - Role(사용자 권한) 정보 조회

    - ```sql
      // \dg 명령어도 동일
      agensdb=# \du
                                      롤 목록
       롤 이름 |                      속성                      | 소속 그룹:
      ---------+------------------------------------------------+------------
       agens   | 슈퍼유저, 롤 만들기, DB 만들기, 복제, RLS 통과 | {}
       test    |                                                | {}
      
      ```

  - Table, View, Sequence 목록 조회

    - ```sql
      agensdb=# \d
      ```

  - 이전 (bufferd) Query 재실행

    - ```
      \g
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
    - Shared buffer : 공유 리소스,  캐시 등등을 저
    - 워크 메모리 : sort, merge, join 등에 사용하는 메모리 사용

  - db 메모리설정

  - pg_hba.conf

    - 접근 허용등 중요한 환경설정

    



