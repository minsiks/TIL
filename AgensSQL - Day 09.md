# 24-04-24 AgensSQL Day 09

## AgensSQL 

### 1. LVM 개념

- 개요

  - LVM(Logical Volume Manager)은 디스크 장치를 논리적으로 관리해주는 기능을 제공

  - 사용자는 LV(Logical Voluem)를 이용하여 물리적인 제약을 벗어남
  - 대표적인 물리적인 제약
    - 파티션 용량 증가 (디스크 용량 증가)
    - 디스크 병합
    - 고장 등 장치 변경 (핫 스와핑)
  - LVM은 디스크(또는 파티션) 상위 계층으로 작동하여 사용자에게 디스크 추상화가 이루어진다
  - 사용자는 물리 디스크의 제약에서 벗어 날 수 있다.
  - 미러링, 스트라이핑 등을 사용하여, 데이터 관리 기능

###  AgensSQL 오전교육 4.24

``` 
> Vaccum
- MVCC
>> lock 기능만 사용하는 dbms? -> Oracle : 레코드의 중복 사본을 생성X
>> FSM => dead tuple을 저장하는 공간
>> Transaction id wraparound?
>> Visibility Map : 
>> Fullvaccum
>>> lock이 걸리며 disk 공간을 확보
>> Autovaccum
>> 쿼리확인 (데드튜플 등)
> WAL
>> segment?
>> 
```

### 2. Vacuum 이해 

- ReaD Commited 모드

  - DBMS마다 가장 많이Default로 적용
  - COMMIT이 완료된 데이터만 SELECT시에 보이는 수준
  - 문제는 트랜잭션 진행 중일 때 SELECT하는 데이터가 동일하다는 보장 X

- MVCC

  - Read Commited 문제 해결
  - 데이터 무결성을 손상시키지 않고 동시에 다른 사용자에 의해 데이터 접근 및 조회 가능
  - PostgreSQL에서 vaccuum이라는 특별한 기능 필요
  - 오라클은 MVCC 구현을 위해 Undo라는 별도 저장소를 통해 변경 전 데이터를 저장

- Vacuum

  - 쉽게 설명하면 디스크 조각모음 또는 Garbage Data(Dead Tuple)를 정리 하는 작접이라 비유
  - PostgreSQL에서는 값이 갱신되거나 삭제할 때 변경 전 데이터를 동일한 데이터 저장소에 그대로 남겨두고 기존 데이터를 Overwrite하거나 정리하지 않고 삭제 대상으로 표기만 한 채 남겨두고 Update 할 데이터는 새로운 Row에 저장
  - 이러한 것으로 UPDATE, DELETE Transaction 이벤트가 많아질 수록 Dead Tuple 수가 늘어나 스캔 범위가 커지면서 Disk I/O가 많이 발생하게 되고 결국 성능 저하의 원인

- 일반 Vacuum, AutoVacuum과 Full Vacuum 차이

  - Vacuum, AutoVacuum을 사행했을 때 OS 디스크의 공간 반환까지는 처리 못하고 FSM에만 반환되어 이 공간을 재사용할 수 있게끔만 처리
    - Online으로 수행되는 동안 DML 가능하며, DDL에 대해서는 제약을 받음
  - Vacuum Full은 OS 디스크의 공간 반환까지 처리
    - 운영중에 X

  > Oracle, MySQL에서는 변경데이터를 Overwrite하고 변경 전 데이터는 Undo라는 별도 데이터파일에 저장을 하며, 또한 Undo영역은 주어진 사이즈에서 Recycle로 Overwrite하게 되어있다.

### 3. WAL, Archive log와 시점복구

> Oracle, PostgreSQL : archive log
>
> MySQL : binary log

- WAL 이란

  - DML,vacuum 등 데이터를 변경하거나 작업 이력이 남는 커맨드에 대해 WAL(Write Ahead Logging) 파일을 남김.
  - WAL 파일을 replay 함으로써 유실된 데이터를 복구
  - WAL 파일은 엔진 디렉토리 내의 pg_wal 디렉토리에 저장
  - ps_wal 디렉토리는 max_wal_size, min_wal_size 파라미터로 pg_wal에 저장할 wal 파일의 개수를 제어
  - max_wal_size를 넘으면 오래된 WAL파일은 지우고 최근파일만 남기게 됨
  - 그렇기 때문에 WAL 파일을 Archiving 해서 저장 필요

- Archiving 설정

  ```
  $ vi postgresql.conf
  archive_mode = on
  archive_command = 'cp %p /home/psql/arch/testdb/%f'
  #restore_command = 'cp /home/psql/arch/testdb%f %p'
  ```

  - 여기서 %p는 WAL파일이 있는 디렉토리, %f는 옮길 WAL 파일의 이름으로 자동 세팅
  - archive_mode를 변경할 때 DB 재시작 필요
