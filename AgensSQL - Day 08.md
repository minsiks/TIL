# 24-04-23 AgensSQL Day 08

## AgensSQL 

### 1. WAL (Write-Ahead Logging)

​	데이터 변경 작업이 디스크에 기록되기 전에 로그에 먼저 기록되는 방식

​	이를 통해 데이터베이스의 일관성과 내구성을 보장

​	WAL은 데이터베이스의 복구 및 장애 대응에 중요한 역할

- 개념
  - WAL data or XLOG 레코드
    - PostgreSQL 내에서 오류를 대비한 모든 수정 사항의 기록 데이터
    - 트랜잭션의 커밋이나 중단 시 WAL segment의 디스크 파일에 기록
    - XLOG 레코드에서 기록된 위치를 나타내기 위해 LSN(Log Sequence Numbr)을 고유 id로 사용
  - checkpoint
    - DBMS가 설정하는 트랜잭션 포인트, 실행 시 checkpoint 이전의 모든 변경사항이 disk에 flush
    - 충돌이나 다운시 마지막 checkpoint 시점(REDO point) 이후의 레코드를 반영하여 복구
    - dirty page의 정리로 disk i/o 부담 감소
    - 발생 조건 (단, 마지막 checkpoint 이후 기록된 WAL이 없으면 발생하지 않음)
      - 이전 checkpoint이후 checkpoint_timeout(5분)이 지난 경우
      - WAL 파일의 크기가 max_wal_size를 초과
      - smart 또는 fast 모드로 DB서버를 종료
      - pg_basebackup 또는 pg_start_backup으로 백업
      - supersuer의 checkpoint 커맨드 실행
  - Archive
    - WAL segment는 재활용되기 때문에 최근의 파일로 교체됨
    - DB를 특정 시점의 특정 상태로 복구하기 위해 WAL 파일을 Archiving 영역으로 복사
  - Replication
    - DB의 복제 또한 WAL 파일을 기반으로 수행됨
  - pg_control 파일
    - DB의 정상 종료 여부, 마지막 체크 포인트 위치 등의 메타데이터를 포함한 파일
- WAL이 없는 경우
  - DB 클러스터에서 TABLE_A페이지를 shared buffer 풀로 로드하고 A튜플을 삽입, 이상태에서는 DB 클러스터에 기록되지 않음
  - 새 튜플을 삽입
  - OS 또는 서버 장애 시, 삽입된 모든 데이터 손실
- WAL이 존재하는 삽입 작업 및 복구
  - 일반적인 WAL을 사용한 삽입과 복구
    - 체크포인터가 체크포인트 레코드인 XLOG 레코드를 현재 가지고 있는 WAL segmentd에 작성
    - 디스크에서 페이지를 버퍼 풀로 로드, XLOG 레코드가 생성되어 lsn_1 위치의 WAL버퍼에 쓰임
    - 커밋 시 커밋 작업의 XLOG 레코드를 생성하고 WAL의 버퍼의 모든 레코드를 WAL segment에 기록
    - DB 재시작 이후 자동으로 REDO 지점의 WAL segment 파일에서 레코드를 읽고 복구
  - 페이지가 손상된 경우 삽입과 복구
    - 체크포인트 이후 첫 페이지에 대한 첫 쓰기는 전체페이지를 포함한 백업 블록으로 취급하고 버퍼로 로드

### 2. System Monitoring Guide

- Monitoring Command
  - TOP
    - TOP 명령을 수행하면 다음과 같이 CPU, Memory, swap, process 목록 등 system의 전반적인 상태를 한눈에 점검할 수 있으며, "shift" Key를 이용하여 %CPU 사용량, 또는 %Memory 사용량에 따라 정렬된 process목록을 조회 할 수 있다.
