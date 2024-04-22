# 24-04-22 AgensSQL Day 07

## AgensSQL 

### 1. Vaccum

- MVCC (Multi Version Concurrency Control)
  - MVCC는 다중 버전 동시성 제어, 이전부터 현재까지의 데이터의 관리 및 제공 기능
  - 변화하는 데이터 중 사용자가 조회를 시작한 시점의 정확한 데이터를 제공
    - Lock 기능만 사용하는 DBMS보다 훨씬 빠르게 동작
- PostgreSQL의 MVCC
  - 테이블 내부 변경 이전과 현재 버전을 데이어를 모두 포함
  - 각 레코드 별로 4byte의 버전정보(XID)를 포함하여 시점을 식별
  - Transaction의 수행 시점의 ID와 레코드의 XID를 비교해 MVCC 구현
  - XID
    - XMIN - insert, update 시점의  Transaction ID 값을 가진 메타데이터
    - xMAX = date, update 시점의 Transaction ID 값을 가진 메타데이터
- PostgreSQL에서 update 동작
  - 페이지에서 사용 가능한 공간을 기록한 FSM(Free Space Map)을 확인
  - FSM의 사용 가능한 공간에 update된 데이터 기록
  - 저장이 완료되면 update 이전의 원본 tuple을 가리키던 포인터를 새로 update된 tuple을 가리키도록 동작
- 문제점
  - Update나 Delete가 빈번한 테이블은 각 데이터의 이전 버전을 모두 저장해야 함
  - 사용하는 공간이 늘어나며, update가 완료되어 참조되지 않는 Dead tuple이어도 스캔을 진행
  - XID는 4byte이고 43억개의 Trasaction 표현 가능, 고갈 문제
- Vaccum의 역할
  - Dead tuple을 정리해 FSM으로 반환, 디스크 공간 확보
  - Transaction ID Wraparound로 인한 오래된 자료 손실 방지
  - 통계 정보 갱신
  - Visibility Map을 갱신하여 index scan 성능 향상
- Vaccum의 동작
  - vaccum
    - 각 페이지에 대해 Dead tuple을 제거하고 live tuple을 조각 모음
    - 필요한 경우 tuple의 과거 txid을 freeze
    - FSM, VM 여러 통계들을 업데이트
  - Full Vaccum
    - 새로운 테이블 파일을 생성 (이 과정에서 테이블 크기 만큼의 용량이 더 필요할 수 있음)
    - 기존 테이블에 AccessExclusiveLock으로 다른 사용자 액세스 방지
    - Live tuple을 새 테이블에 복사
    - 이전 테이블 제거, 인덱스 재구축, 통계 업데이트
    - 일반 Vaccum에 비해 디스크 공간까지 확보가 가능하지만, Lock과 용량 문제가 존재
  - Autovaccum
    - 특정 조건을 만족할 시, 일반 Vaccum 작업을 백그라운드에서 수행
    - 일반적으로 Dead tuple의 누적치, 테이블이나 tuple age의 누적치를 트리거로 사용

### 2. PostgreSQL 성능

- 아키텍처

  - Shared Memory
    - Shared Buffer
      - DISK I/O 최소화 목적
      - 많은 데이터를 Buffer를 통해 빠르게 접근 가능
      - 많은 사용자가 동시 접속시 경합을 최소화 가능
      - 자주 사용되는 블록들은 최대한 오랫동안 Buffer에 상주
      - 권장 크기는 물리메모리 * 0.25로 설정
      - Vaccum은 일종의 Garbage collector 역할, Shared Buffer 내 불필요한 Page(Dead tuple)를 찾아내어 Buffer에서 제거하는 작업 수행
      - Vaccum 명령시 ANALYZE 옵션을 더하면 Table, Index 통계 정보 수집과 Table 재구성 작업을 수행
    - WAL Buffer
      - Database의 변경 사항을 잠시 저장하는 Buffer로 주기적으로 WAL파일에 저장
      - WAL 파일은 Database 복구에 있어서 매우 중요하게 사용
      - WAL Buffer와 관련된 Parameter는 성능의 관점에서도 매우 중요

- Process 유형

  - Postmaster Process

    - PostgreSQL을 기동하면 제일 먼저 수행되는 Process
    - 복구작업, Shared Memory 초기화, Background Process생성을 순차적으로 수행
    - Client Process의 접속 요청이 있을 때 Background Process를 생성

  - Background Process

    PostSQL운영에 필요한 Process등을 생성

    - Logger : Error Message를 Log File 기록
    - Checkpointer : checkpoint 발생 시 dirty buffer를 file에 기록
    - Writer : 주기적으로 dirty buffer를 file에 기록
    - WAL writer : WAL buffer 내용을 WAL 파일에 기록
    - Autovacuum launcher : Vacuum이 필요한 시점에 Autovacuum worker Process를 생성
    - Archiver : Archive log 모드일 때 WAL File을 지정된 Directory에 복사
    - Stats collector : 테이블, 세션과 관련된 통계 정보를 수집

  - Backend Process

    사용자 Query 요청에 대한 응답, 처리, 결과 전송을 위한 Process

    Client Process로부터 요청이 들어오면 Postmaster Process가 생성

  - Memory Parameter
    - Work_mem : 정렬, bitmap, Hash Join, Merge Join 작업 시 사용되는 공간
      - 기본값은 4MB 
    - Maintenance_work_mem : Vacuum, Index 생성 작업시 사용
      - 기본값은 64MB
    - Temp_buffer : Temporary Table 을 저장하기 위한 공간

#### 4.22 PostgreSQL 오전 교육

```
### 아키텍처
-Cost BAsed Optimizeing
-With문 사용과 heap table 관계
	WITH sel_by_region AS ( SELECT Region, Sum(salesAmount) AS TOTALSALES
	FROM SALES
	GROUP BY REGION
	)
	SELECT Region, TotalSales
	FROM sel_by_region
	where TotalSales > 10000;
-Tablespace : 논리적인 저장 공간
>> 오라클 저장공간 크기 설정가능 != postgre,mysql 디렉토리 크기가 저장공간
-file per table
>mysql은 default date directory 사이징 불가
>mysql 파라미터
>> open files / max user processes
- 데이터베이스 swap
- hang 현상
- Dirty Page
>> overcommit_memory,ratio : 기본값보다는 보수적으로 설정요망

```

### 3. SQL Guide

- BASIC Rule

  - SQL 구문중에 Reserved Word는 대문자로 작성

    ```
    SELECT
      col1
    , col2
    , STR_TO_DATE(col3, '%Y%m%d') AS in_date
    FROM
      table
    WHERE
      col1 = 'key'; 
    ```

  - SQL 구문은 Block 단위로 들여쓰기를 하여 가독성을 높인다

  - Host 변수는 Column과 구분하기 위해서 Prefix를 사용, Column명은 한글명과 대응되는 영문으로 사

    

    
