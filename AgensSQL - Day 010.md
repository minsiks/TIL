# 24-04-25 AgensSQL Day 10

## AgensSQL 

### 1. Vacuum 수행관리

-  필요성

  - Auzre Database for PostgreSQL Guide 문서에 따르면 dead tuples로 인해 아래와 같은 문제가 발생 할 수 있다
    - Data bloat, such as larger databases and tables. (데이터베이스와 테이블들이 커지는 데이터 팽창)
    - Larger subopimal indexes. (불필요한 혹은 부적절한 인덱스 증가)
    - Increased I/O. (I/O 증가)
  - 문제 해결위해 수동 또는 자동 VACUUM 기능 사용 권장
  - postgresql.conf 파일에서 설정을 확인 가능

- Autovacuum 파라미터

  - autovacuum_vacuum_scale_factor
    - Dead tuple의 %를 계산하여 autovacuum을 실행하지만 0으로 설정할 시 autovacuum_vacuum_threshold 수에 도달 시 실행
  - autovacuum_vacuum_cost_limit
    - 다른 설정들과 함께 복합적으로 동작, Autovacuum이 한 번 동작할 때의 동작 시간을 결
  - autovacuum_analyze_scale_factor 
    - AgensSQL은 autovacuum 데몬을 통해 주기적으로 분석 데이터를 수집
  - autovacuum_work_mem
    - Autovacuum이 동작 할 때, autovacuum_work_mem이 설정된 메모리를 이용
  - autovacuum_max_workers
    - 동시에 동작 가능한 Autovacuum의 프로세스 갯

- 발생 할 수 있는 이슈와 관리 방안

  - Dead Tuple 관리

    ```sql
    /* dead tuple 이 1000 개 이상인 테이블의 dead tuple / live tuple 의 비율을 계산하여 출력 쿼리 */
    SELECT relname, n_live_tup, n_dead_tup, n_dead_tup / (n_live_tup::float) as ratio
    FROM pg_stat_user_tables WHERE n_live_tup > 0 AND n_dead_tup > 1000;
    ```

    ```sql
    /* vacuum의 최근 수행 이력 조회 쿼리 */
    SELECT relname, last_vacuum, last_autovacuum, last_analyze, last_autoanalyze
    FROM pg_stat_user_tables
    ORDER BY relname;
    ```

    ```sql
    /* dead tuple 식별용 샘플 쿼리 */
    SELECT relname, n_dead_tup, n_live_tup, (n_dead_tup / n_live_tup)
    AS DeadTuplesRatio, last_vacuum, last_autovacuum
    FROM pg_stat_user_tables
    WHERE relname = 'sample_table' /* 본인의 테이블명 */
    ```

- Autovaccum 정상 동작 테스트

  - 데이터 삽입 및 수정

  ```
  create table test (a int, b int);
  alter table test set (autovacuum_enabled = false); /* autovacuum 비활성화 */
  insert into test select generate_series, generate_series from generate_series(1, 1000000);
  \dt+ test
  /* table 크기 : 3562 kb(10만건) / 총 10만개(10만:live tuple)의 dead tuple 생성 */
  update test set b = b + 1;
  \dt+ test
  /* table 크기 : 7104 kb(20만건) / 총 20만개(10만:dead tuple + 10만:live tuple)의 dead tuple생성 */
  update test set b = b + 1;
  \dt+ test
  /* table 크기 : 10 MB(30만건) / 총 30만개(10만 + 10만):dead tuple + 10만:live tuple)의 dead tuple 생성 */
  ```

  - Dead Tuple, Live Tuple 확인

  ```
  adc=# select relname, n_live_tup, n_dead_tup, n_dead_tup / (n_live_tup::float) as ratio
  FROM pg_stat_user_tables
  WHERE n_live_tup > 0 AND n_dead_tup > 1000
  ORDER BY ratio DESC;
  ```

  - AutoVacuum 동작 쿼리 확인

  ```
  /* 동작을 하려면 VACUUM ANALYZE '테이블명' */
  
  adc=#SELECT relname, last_vacuum, last_autovacuum, last_analyze, last_autoanalyze
  FROM pg_stat_user_table
  order by relname;
  ```

  - Dead Tuple, Live Tuple 확인

  ```
  adc=# select relname, n_live_tup, n_dead_tup, n_dead_tup / (n_live_tup::float) as ratio
  FROM pg_stat_user_tables
  WHERE n_live_tup > 0
  ORDER BY ratio DESC;
  ```

### 24.04.25 AgensSQL 오전 교육

```
> 모니터링
평균적인 수치를 파악하여 문제를 파악하기 위함
> top 라인
>> 실시간으로 대략적인 정보 확인 가능
>> zombie를 중점적으로 확인
>> st VM환경에서 확인필요
>> interupt는 핵심적인 키워드 - si
>> uptime을 이용하여 장애 시점을 추정가능
>> ni -> nice : 우선순위가 높은 프로세스들에게 cpu 사용을 양보한 Process의 비중 
>> Mem 중에선 free 메모리가 중요
>> Swap은 가급적 사용하지 않는 것이 default (SWAP 메모리?)
> sar 명령어
>> sysstat 패키지 설치 필요
> Linux Cache System Layout
>> dirty cache 가 많아지면 dirty page ratio 가 증가
>> 메모리 구조 필요
> Overcommit & OOM Killer 이해 필요
> LVM
>> 파티셔닝
>> raid의 이해
나스?
> OS 명령어
```

### 2. postgreSQL Data Access 및 Transaction

- Connection

![image-20240425141504989](C:\Users\min\Documents\TIL\assets\image-20240425141504989.png)

1. 사용자가 서버로 연결 요청
2. Postmaster 프로세스는 자신을 fork()하여 자식 프로세스인 postgre를 생성
3. postgres 프로세스가 Client와 연결되면 postmaster 프로세스는 Client와 연결 해제, Client는 postgres 프로세스에 SQL Quenry 전달
4. postgres 프로세스는 전달받은 Query를 수행한 후 Client에 결과 반환

```
[커넥션 생성 과정] 
postmaster 프로세스가 오라클의 Listener 처럼 개별 프로세스를 부여하는 데몬 프로세스이며, Client로부터 새로운 연결 요청을 기다립니다. 
새로운 Client 연결 요청에 대한 포트(5432)를 인증하고 부모 프로세스(자신)를 fork()하여 자식 프로세스인 postgres 를 생성합니다. 이때 각각의 프로세스에는 work_mem 같은 Local Memory가 개별 할당됩니다. (쿼리 실행에는 일부 로컬 메모리가 필요함)

postgres 주요 파라미터
      work_mem 파라미터: 쿼리 작업 영역. 정렬(order by, distinct) 작업, 해시 조인 및 Merge 조인 등을 할 때 사용하는 임시 메모리. (기본 설정: 4 MiB)
                                  위 작업이 지정한 값보다 많은 양의 메모리가 요구되고, 빈번하게 일어난다면 값을 조정할 필요가 있다.
                                   * 계산식 :  (OS 캐시 메모리 / 연결 ) * 0.5
      maintenance_work_mem 파라미터: 유지 관리 작업에 사용할 최대 메모리. Vacuum과 Create Index 작업 시에 사용되는 공간이다. (기본 설정: 64 MiB)
                                                    값은 시스템 메모리의 5%가 적당하다. (work_mem 보다 항상 높아야 한다)
      temp_buffers 파라미터: Temporary 테이블을 저장하기 위한 공간이다. (기본 설정: 8 MiB)

```

- SQL 수행 과정

![image-20240425142735286](C:\Users\min\Documents\TIL\assets\image-20240425142735286.png)

1. 파서 단계에서는 클라이언트로부터 질의 요청이 들어오면 구문 분석 과정을 통해 Parser Tree를 생성
2. 의미 분석 과정을 통해 새로운 트랜잭션을 시작하고 Query Tree를 생성
3. 이후 서버에 정의된 Rule(시스템 카탈로그에 저장됨)에 따라 Query Tree가 재 생성
4. 실행 가능한 여러 수행 계획 중 가장 최적화된 Plan Tree를 생성
5. 서버는 이를 수행하여 요청된 질의에 대한 결과를 클라이언트로 전달

- DML 수행 과정 (SELECT)

![image-20240425143121322](C:\Users\min\Documents\TIL\assets\image-20240425143121322.png)

1. client가 postgres 프로세스로 쿼리를 전달
2. Postgres 프로세스는 Database의 Shared Buffers에서 Data를 찾고, 존재한다면 값을 전달
3. Shared Buffers에 데이터가 없다면, 디스크(Data 파일)에서 원하는 Block을 Shared Buffers로 캐싱합니다. 이때 물리적인 I/O가 발생합니다.

- DML 수행 과정 (INSERT)

![image-20240425143330627](C:\Users\min\Documents\TIL\assets\image-20240425143330627.png)

1. client가 postgres 프로세스로 쿼리를 전달
2. postgres 프로세스는 데이터를 Shared Memory에 전달
3. WAL Buffers로 데이터가 쓰여집니다. (특정 시점에 WAL Writer 프로세스를 통해 Disk에 WAL Files로 저)
4. 데이터가 WAL로 전송되면 PostgreSQL은 Shared Buffers에 있는 캐시된 블록 버전을 변경
5. Shared Buffers에서 변경된 Data는 BG Writer와 Checkpointer 프로세스가 정기적으로 일정량의 Dirty Block을 Data에 Write

- DML 수행과정 (UPDATE,DELETE)

![image-20240425144548457](C:\Users\min\Documents\TIL\assets\image-20240425144548457.png)

1. client가 postgres 프로세스로 쿼리를 전달합니다.

2. Postgres 프로세스는 Database의 Shared Buffers에서 Data를 찾고, 존재한다면, WAL Buffers로 데이터를 쓴 후, 캐시된 블록 버전을 변경합니다. (MVCC)

  \- Shared Buffers에서 Data를 찾지 못할 경우, Disk에서 Shared Buffers로 Data를 캐싱 후 진행합니다.

  \- WAL Buffers의 Data는 특정 시점에 WAL Writer 프로세스를 통해 Disk에 WAL Files로 저장됩니다.

3. Shared Buffers에서 변경된 Data는 BG Writer 와 Checkpointer 프로세스가 정기적으로 일정량의 Dirty Block를 Data Files에 Write합니다.
