# 24-04-19 AgensSQL Day 06

## AgensSQL 

### 1. WAL(Write Ahead Log)

- WAL이란

  - WAL (Write-Ahead Logging)은 데이터 무결성을 보장하는 표준 방법, 데이터 변경 작업이 디스크에 기록되기 전에 로그에 먼저 기록되는 방식
  - WAL을 사용하면, 데이터 변경 작업이 디스크에 반영되기 전에 Disk 장애 발생 시 변경 사항에 대한 로그(WAL)가 존재 하기 때문에 해당 명령을 다시 수행하여, 변경 내역 복구가 가능하다.
  - 트랜잭션에 의해 변경된 내용이 모든 데이터 파일(Disk Tmrl)이 아니라 WAL 파일만 디스크에 저장하면되기 때문에 Disk I/O가 줄어든다.

- 사용중인 WAL segment (=WAL File)Size 조회

  - 디렉토리 조회 (binary 형태의 파일)

    ```
    cd $PGDATA/pg_wal
    ll
    ```

  - DB 조회

    ```
    select fname as file_name,
    ((pg_stat_file('pg_wal/'||fname)).size)/1024/1024 as size_mb,
    (pg_stat_file('pg_wal/'||fname)).isdir as isdir
    from pg_ls_dir('pg_wal') as t(fname);
    ```

    **pg_stat_file() 파일에 대한 메타데이터를 반환하는 시스템 함수

    **pg_ls_dir() 디렉토리 조회
