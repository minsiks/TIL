# 24-05-22 AgensSQL Day 25

## AgensSQL 

### AgesnSQL 24-06-27 오전 교육

## 1. Partial Index (부분 인덱스)

[11.8. Partial Indexes](https://www.postgresql.org/docs/14/indexes-partial.html)

<aside> 💡 Partial Index란

- 전체 테이블이 아닌, 특정 조건을 만족하는 데이터에만 인덱스를 생성
- 특수한 기능이지만 유용한 여러 상황이 존재
- 디스크 공간 절약, 인덱스 검색 성능 향상 기여
- 주요 이유
  - 조건에 맞는 부분만 인덱싱, 전체 테이블을 인덱싱하는 것보다 인덱스 크기감소
  - 저장 공간을 절약하고 인덱스 생성 및 유지 관리 시간 감소
  - 공통 값의 인덱싱을 피하기 위함
  - 특정 조건을 만족하는 데이터 검색 시 유용하게 사용
  - 필요한 데이터만 인덱싱 조회 시 불필요한 데이터 접근을 최소화, 조회 속도 향상 </aside>

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/fa64aa7d-9054-4148-8d2a-f4be4bc9e749/a809d5ad-3738-4015-bdfc-7e23176462e6/Untitled.png)

### 1-1. Index 생성 문법 (단일, 다중 컬럼 인덱스 생성)

```sql
-- index 생성
-- 단일 인덱스
CREATE INDEX 인덱스이름 ON 테이블이름(컬럼명)
ex) CREATE INDEX idx_return_state ON rental(return_state)

-- 다중 컬럼 인덱스
CREATE INDEX 인덱스이름 ON 테이블이름(컬럼명1, 컬럼명2 ... )
ex) CREATE INDEX idx_unq_rental_rental_date_inventory_id_customer_id 
		ON rental(rental_date, inventory_id, customer_id);

-- partial Index 생성
-- 단일 index
CREATE INDEX 인덱스이름 ON 테이블이름(컬럼명) WHERE 조건 절
ex) CREATE INDEX idx_return_state_false ON rental(return_state) 
		WHERE return_state is not ture;

--다중 컬럼 Index
CREATE INDEX 인덱스 이름 ON 테이블이름(컬럼명1. 컬럼명2 ...) WHERE 조건 절
ex) CREATE idx_unq_rental_rental_date_inventory_id_customer_id 
		ON rental(rental_date, inventory_id, customer_id) WHERE return_date is null;
```

### 1-2. 사용 예시

- 공통 값 제외 Partial Index 생성 (공통 값의 인덱싱을 피하기 위함)
  - 공통 값(테이블의 약 10% 이상을 차지하는 값) 인덱싱을 피하기 위해 (인덱스 사용 불필요)
  - Index 크기 감소 → 쿼리 속도 향상
  - 모든 Index를 업데이트 할 필요 X → 테이블 업데이트 작업 속도 향상

```sql
-- 테이블 예시
CREATE TABLE access_log(
	url varchar,
	client_ip inet,
	...
);

-- Network Address Types : ‘inet’ 7 or 19 bytes, IPv4 and IPv6 hosts and networks
-- postgreSQL에서 네트워크 정보를 저장하기 위해 고안된 데이터 타입

--Index 생성 목적
-- 테이블 활용도 : Web Server Access log를 저장
-- 가정 : 
--        대부분의 Web Server 접속 로그는 내부에서 발생, 일부는 원격 접속
--        access_log 테이블은 IP별 검색이 주로 원격 접속에 관한 데이터 조회 (내부 접속 IP 범위는 Index 불필요)

-- Index 생성 예시
CREATE INDEX access_log_client_ip_ix ON access_log(client_ip)
WHERE NOT (client_ip > inet '192.168.100.0' AND
					 client_ip < inet '192.168.100.255');
					 
-- 인덱스 사용 쿼리
SELECT *
FROM access_log
WHERE url '/index.html' AND client_ip = inet '212.78.10.32';

-- Index 미사용 쿼리
SELECT *
 FROM access_log
 WHERE url='/index.html' AND client_ip = inet '192.168.100.23';
```

- 자주 조회되는 값 Partial Index 생성
  - 테이블에서 비교적 작은 부분이지만 자주 조회되는 값을 Partial Index 생성

```sql
-- Index 생성 목적
--  테이블 활용도 : 청구된 주문, 청구되지 않는 주문이 모두 포함된 테이블
--  가정 : 청구되지 않은 주문(unbilled) 이 테이블에 적은 행을 가지지만 자주 조회
--  이점 : 청구되지 않은 주문 행에만 Index를 생성 하여 성능 향상

-- Index 생성 예시
CREATE INDEX orders_unbilled_index ON orders (order_nr)
		WHERE billed is not true;

-- Index 사용 쿼리
SELECT * FROM orders WHERE billed is not true AND order_nr < 10000;

-- Index 사용 쿼리(Index 컬럼 미포함)
SELECT * FROM orders WHERE billed is not true AND amount > 50000.00;
-- 인덱스는 적용될 수 있지만 최적화된 형태 X
-- amount 컬럼에 대한 부분 인덱스를 사용하는 것이 더 효율적
-- billed가 true가 아닌 레코드가 적다면 현재 인덱스도 어느 정도 효과적

-- Index 미사용 쿼리
SELECT * FROM orders WHERE order_nr = 3501;
```

- Partial Unique Index 설정

```sql
--Index 생성 목적
-- 테이블 활용도 : 테스트 결과 설명 테이블
-- 가정 : subject, target에 대한 실패한(unsuccessful) 값이 많은 상태에서 하나의 성공한 (successful) 값만 조회
-- 이점 : 실패한 테스트가 많고 성공한 테스트가 적을 때 효율

CREATE TABLE tests (
    subject text,
    target text,
    success boolean,
    ...
);

CREATE UNIQUE INDEX tests_success_constraint ON tests (subject, target)
    WHERE success;
--제한 사항이 있는 고유한 부분 인덱스를 생성하여 열에 하나의 Null만 허용하는 것도 가능 IS NULL
```

### 1-3. Partial Index VS 일반적인 Index 비교

- nshop 데이터베이스 비교

```sql
nshop=# \\d t_login_hist
                      Table "nshop.t_login_hist"
   Column    |         Type          | Collation | Nullable | Default
-------------+-----------------------+-----------+----------+---------
 usr_id      | character varying(20) |           | not null |
 login_dt    | character(8)          |           | not null |
 seq_no      | bigint                |           | not null |
 login_tm    | character(6)          |           | not null |
 login_gw_ip | character varying(20) |           |          |
 logout_dt   | character(8)          |           |          |
 logout_tm   | character(6)          |           |          |
 logout_st   | character(1)          |           |          |
 rmrk        | character varying(50) |           |          |

nshop=# select count(*) from t_login_hist ;
 count
-------
 11828
(1 row)

nshop=# select * from t_login_hist;
        usr_id        | login_dt | seq_no | login_tm |   login_gw_ip   | logout_dt | logout_tm | logout_st | rmrk
----------------------+----------+--------+----------+-----------------+-----------+-----------+-----------+------
 12345                | 20180615 |      1 | 112753   | 127.0.0.1       |           |           |           |
 12345                | 20180615 |      2 | 112759   | 127.0.0.1       |           |           |           |
 12345                | 20180615 |      3 | 125716   | 127.0.0.1       |           |           |           |
 12345                | 20180615 |      4 | 125734   | 127.0.0.1       |           |           |           |
 12345                | 20230202 |      1 | 175833   | 0:0:0:0:0:0:0:1 |           |           |           |
 14242                | 20191011 |      1 | 090407   | 0:0:0:0:0:0:0:1 |           |           |           |
 1q2w3e4r             | 20180625 |      1 | 140431   | 127.0.0.1       |           |           |           |
 2757608849           | 20230424 |      1 | 053330   | 127.0.0.1       |           |           |           |
 2757608849           | 20230424 |      2 | 053337   | 127.0.0.1       |           |           |           |
 2757608849           | 20230424 |      3 | 053350   | 127.0.0.1       |           |           |           |
 2757608849           | 20230424 |      4 | 053536   | 127.0.0.1       |           |           |           |
 2757608849           | 20230424 |      5 | 053557   | 127.0.0.1       |           |           |           |
 2757608849           | 20230424 |      6 | 053839   | 127.0.0.1       |           |           |           |
 2757608849           | 20230424 |      7 | 053845   | 127.0.0.1       |           |           |           |
 2757608849           | 20230424 |      8 | 053939   | 127.0.0.1       |           |           |           |
 2757608849           | 20230424 |      9 | 054054   | 127.0.0.1       |           |           |           |

nshop=# EXPLAIN ANALYZE select * from t_login_hist where login_tm ='123848' and  login_gw_ip ='127.0.0.1';
                                                QUERY PLAN
----------------------------------------------------------------------------------------------------------
 Seq Scan on t_login_hist  (cost=0.00..301.42 rows=1 width=234) (actual time=0.022..3.619 rows=1 loops=1)
   Filter: ((login_tm = '123848'::bpchar) AND ((login_gw_ip)::text = '127.0.0.1'::text))
   Rows Removed by Filter: 11827
 Planning Time: 3.713 ms
 Execution Time: 3.680 ms
(5 rows)

nshop=# EXPLAIN ANALYZE select * from t_login_hist where login_tm ='123848' and  login_gw_ip ='59.10.223.239';
                                                QUERY PLAN
----------------------------------------------------------------------------------------------------------
 Seq Scan on t_login_hist  (cost=0.00..301.42 rows=1 width=234) (actual time=1.986..1.987 rows=0 loops=1)
   Filter: ((login_tm = '123848'::bpchar) AND ((login_gw_ip)::text = '59.10.223.239'::text))
   Rows Removed by Filter: 11828
 Planning Time: 1.956 ms
 Execution Time: 2.102 ms

nshop=# CREATE INDEX access_log_cient_ip_ix ON t_login_hist(login_gw_ip)
WHERE login_gw_ip != '0:0:0:0:0:0:0:1';

nshop=# EXPLAIN ANALYZE select * from t_login_hist where login_tm ='123848' and  login_gw_ip ='59.10.223.239';
                                                           QUERY PLAN
---------------------------------------------------------------------------------------------------------------------------------
 Bitmap Heap Scan on t_login_hist  (cost=4.79..113.84 rows=1 width=234) (actual time=0.108..0.110 rows=0 loops=1)
   Recheck Cond: ((login_gw_ip)::text = '59.10.223.239'::text)
   Filter: (login_tm = '123848'::bpchar)
   Rows Removed by Filter: 67
   Heap Blocks: exact=6
   ->  Bitmap Index Scan on access_log_cient_ip_ix  (cost=0.00..4.79 rows=67 width=0) (actual time=0.046..0.046 rows=67 loops=1)
         Index Cond: ((login_gw_ip)::text = '59.10.223.239'::text)
 Planning Time: 0.205 ms
 Execution Time: 0.146 ms
(9 rows)
```

## 2. BRIN(Block Range Index)

<aside> 💡 brin index란?

[postgreSql using brin index란?](https://junghyungil.tistory.com/221)

- BRIN 인덱스는 데이터를 블록 단위로 그룹화하고 각 블록에 대한 최소값과 최대값을 저장
- 블록은 데이터를 물리적으로 저장하는 단위를 나타냄.
- PostgreSQL에서 블록은 일반적으로 8KB의 고정 크기로 정의.
- 데이터베이스 시스템은 이러한 블록을 사용하여 디스크 공간을 관리하고 데이터를 읽거나 쓰는 작업을 수행
- BRIN 인덱스는 이러한 블록을 기반으로 동작
- 메타데이터를 블록 단위로 묶어서 인덱스를 생성하고 관리
  - BRIN 인덱스의 메타데이터는 PostgreSQL pg_class 시스템 카탈로그 테이블에 저장
- 각 블록에 대해 해당하는 최소값과 최대값을 저장
- 블록은 일반적으로 정렬된 데이터 범위를 포함하며, BRIN 인덱스는 이러한 블록을 활용하여 성능을 향상
- 시간 기반 데이터의 경우, 블록은 시간 범위에 해당하게 되고 각 블록에는 해당 시간 범위 내의 최소값과 최대값이 저장
- 블록을 사용함으로써 인덱스의 크기를 줄이고, 특정 시간 범위에 대한 쿼리를 빠르게 처리할 수 있도록 최적화
- 시간에 따라서 정렬해야 하는 대용량 시계열 데이터를 검색하는데 유용
  - 주로 시계열 데이터, 로그 데이터, 이벤트 데이터 등이 포함
- BRIN 인덱스가 시간 기반 데이터에 어떻게 적합한지에 대한 구체적인 설명
  1. **시간 범위 스캔 최적화** : 시간 기반 데이터에서는 주로 최신 데이터에 대한 쿼리 또는 특정 시간 범위의 데이터에 대한 쿼리가 자주 수행. BRIN 인덱스는 각 블록의 최소 및 최대 값만을 저장하므로, 특정 시간 범위에 대한 스캔 작업을 효과적으로 최적화
  2. **인덱스 크기 최소화** : BRIN 인덱스는 블록 범위에 대한 최소 및 최대 값만을 저장하기 때문에, 인덱스 크기가 다른 인덱스 유형에 비해 상대적으로 작음. 이는 디스크 공간을 절약하고 메모리 효율성 향상
  3. **대규모 테이블 성능** 향상 : BRIN 인덱스는 대량의 데이터를 다루는 테이블에서 성능을 향상. 특히 데이터가 시간에 따라 정렬되어 있고 쿼리 패턴이 최신 데이터에 집중되는 경우에 이점 </aside>

B-tree index 구조

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/fa64aa7d-9054-4148-8d2a-f4be4bc9e749/854c11d9-4df5-4fe2-8c95-16f83a98e7e3/Untitled.png)

BRIN index 구조

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/fa64aa7d-9054-4148-8d2a-f4be4bc9e749/cec9ba76-99c5-42e2-b070-81de152d5653/Untitled.png)

- BRIN, B-TREE 비교
  - BRIN INDEX를 적절한 사용시 B-tree 보다 쿼리 성능 우위
  - B-tree보다 적은 디스크 공간을 차지
  - 인덱스 생성 속도도 BRIN이 더 빠름.
- 시간 범위

### 2-1. 쿼리 비교 예

[PostgreSQL BRIN Indexes: Big Data Performance With Minimal Storage | Crunchy Data Blog](https://www.crunchydata.com/blog/postgresql-brin-indexes-big-data-performance-with-minimal-storage)

- 샘플 데이터 만들기
  - 2초마다 데이터를 스캔하는 애플리케이션이 있다고 가정
    - 스캔 값과 기록된 시간을 저장한다고 가정

```sql
-- UNLOGGED테이블 :테스트 목적으로 WAL을 생성하지 않아 데이터를 로드, 성능 시간 향상
CREATE UNLOGGED TABLE scans (
    id int GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    scan float NOT NULL,
    created_at timestamptz NOT NULL
);

INSERT INTO scans (scan, created_at)
SELECT random(), x
FROM generate_series('2024-01-01 0:00'::timestamptz,
('2024-06-27 20:33:20'::timestamptz, '2 seconds'::interval) x;

INSERT 0 7726601

--  7,726,601 건의 데이터 생성

SELECT count(*) FROM scans;

   count   
-----------
 7726601
```

- 인덱스 X

```sql
--일마다 평균 스캔 값을 조회하는 쿼리를 사용

EXPLAIN ANALYZE SELECT date_trunc('day', created_at), avg(scan)
FROM scans
WHERE created_at BETWEEN '2024-02-01 0:00' AND '2024-02-28 11:59:59'
GROUP BY 1
ORDER BY 1;

                                                                                  QUERY PLAN
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Finalize GroupAggregate  (cost=144806.21..286739.22 rows=1147515 width=16) (actual time=7919.254..8000.315 rows=28 loops=1)
   Group Key: (date_trunc('day'::text, created_at))
   ->  Gather Merge  (cost=144806.21..264745.19 rows=956262 width=40) (actual time=7868.799..7995.708 rows=56 loops=1)
         Workers Planned: 2
         Workers Launched: 2
         ->  Partial GroupAggregate  (cost=143806.18..153368.80 rows=478131 width=40) (actual time=7784.019..7865.304 rows=19 loops=3)
               Group Key: (date_trunc('day'::text, created_at))
               ->  Sort  (cost=143806.18..145001.51 rows=478131 width=16) (actual time=7742.331..7778.557 rows=396000 loops=3)
                     Sort Key: (date_trunc('day'::text, created_at))
                     Sort Method: quicksort  Memory: 51769kB
                     Worker 0:  Sort Method: quicksort  Memory: 53072kB
                     Worker 1:  Sort Method: quicksort  Memory: 25kB
                     ->  Parallel Seq Scan on scans  (cost=0.00..98701.58 rows=478131 width=16) (actual time=3673.404..7669.511 rows=396000 loops=3)
                           Filter: ((created_at >= '2024-02-01 00:00:00+09'::timestamp with time zone) AND (created_at <= '2024-02-28 11:59:59+09'::timestamp with time zone))
                           Rows Removed by Filter: 2179534
 Planning Time: 11.987 ms
 Execution Time: 8005.087 ms
(17 rows)

Time: 8044.246 ms (00:08.044)

-- Time: 8044.246 ms (00:08.044)
```

- B-TREE INDEX 사용

```sql
CREATE INDEX scans_created_at_idx ON scans (created_at);

EXPLAIN ANALYZE SELECT date_trunc('day', created_at), avg(scan)
FROM scans
WHERE created_at BETWEEN '2024-02-01 0:00' AND '2024-02-28 11:59:59'
GROUP BY 1
ORDER BY 1;

                                                                              QUERY PLAN
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
 GroupAggregate  (cost=161218.37..187037.46 rows=1147515 width=16) (actual time=4107.759..4319.894 rows=28 loops=1)
   Group Key: (date_trunc('day'::text, created_at))
   ->  Sort  (cost=161218.37..164087.16 rows=1147515 width=16) (actual time=4099.407..4166.230 rows=1188000 loops=1)
         Sort Key: (date_trunc('day'::text, created_at))
         Sort Method: quicksort  Memory: 104840kB
         ->  Index Scan using scans_created_at_idx on scans  (cost=0.43..45720.52 rows=1147515 width=16) (actual time=6.659..3817.977 rows=1188000 loops=1)
               Index Cond: ((created_at >= '2024-02-01 00:00:00+09'::timestamp with time zone) AND (created_at <= '2024-02-28 11:59:59+09'::timestamp with time zone))
 Planning Time: 22.413 ms
 Execution Time: 4333.798 ms
(9 rows)

Time: 4402.111 ms (00:04.402)

SELECT pg_size_pretty(pg_relation_size('scans_created_at_idx'));
 pg_size_pretty
----------------
 166 MB
(1 row)

-- Time: 4402.111 ms (00:04.402)
-- 인덱스 크기 : 166 MB
```

- BRIN INDEX

```sql
DROP INDEX scans_created_at_idx;
CREATE INDEX scans_created_at_brin_idx ON scans USING brin(created_at);

EXPLAIN ANALYZE SELECT date_trunc('day', created_at), avg(scan)
FROM scans
WHERE created_at BETWEEN '2024-02-01 0:00' AND '2024-02-28 11:59:59'
GROUP BY 1
ORDER BY 1;
                                                                                 QUERY PLAN
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 GroupAggregate  (cost=185346.48..211165.57 rows=1147515 width=16) (actual time=1242.148..1442.523 rows=28 loops=1)
   Group Key: (date_trunc('day'::text, created_at))
   ->  Sort  (cost=185346.48..188215.27 rows=1147515 width=16) (actual time=1221.465..1284.963 rows=1188000 loops=1)
         Sort Key: (date_trunc('day'::text, created_at))
         Sort Method: quicksort  Memory: 104840kB
         ->  Bitmap Heap Scan on scans  (cost=304.73..69848.63 rows=1147515 width=16) (actual time=3.702..784.200 rows=1188000 loops=1)
               Recheck Cond: ((created_at >= '2024-02-01 00:00:00+09'::timestamp with time zone) AND (created_at <= '2024-02-28 11:59:59+09'::timestamp with time zone))
               Rows Removed by Index Recheck: 17760
               Heap Blocks: lossy=7680
               ->  Bitmap Index Scan on scans_created_at_brin_idx  (cost=0.00..17.86 rows=1164007 width=0) (actual time=2.938..5.676 rows=76800 loops=1)
                     Index Cond: ((created_at >= '2024-02-01 00:00:00+09'::timestamp with time zone) AND (created_at <= '2024-02-28 11:59:59+09'::timestamp with time zone))
 Planning Time: 17.492 ms
 Execution Time: 1461.027 ms
(13 rows)
Time: 1554.500 ms (00:01.554)

SELECT pg_size_pretty(pg_relation_size('scans_created_at_brin_idx'));
 pg_size_pretty
----------------
 32 kB
(1 row)

-- Time: 1554.500 ms (00:01.554)
-- 인덱스 크기 : 32 kB
```

## 3. Partial Index, BRIN 사용시 유의점

### 3-1 Partial Index

- 인덱스 관리

  :

  - 인덱스를 생성할 때 설정한 조건이 변경되면 인덱스를 재생성

- 복잡한 인덱스 설계

  :

  - 조건이 복잡하거나 자주 변경되는 경우 인덱스 설계 및 관리 어려움

### 3-2 BRIN

- 데이터 분포

  :

  - BRIN 인덱스는 데이터가 자연스럽게 정렬된 경우에 가장 효과적. 데이터가 무작위로 삽입되면 성능이 저하

- 정확성 부족

  :

  - BRIN 인덱스는 블록 범위의 최소 및 최대 값만 저장, 특정 값을 찾는 세밀한 조회 성능은 B-tree 인덱스보다 저하 가능성

- 재검사 필요

  :

  - BRIN 인덱스를 사용한 조회는 범위를 좁히는 데 도움이 되지만, 최종 결과 집합을 얻기 위해 추가적인 검사가 필요 할 수 있음. 이는 추가적인 I/O 유발

## 4. Partial Index, BRIN 실제 사용사례

### 4-1. Partial Index

- 사례 1: 최근 데이터 조회 최적화
  - **상황**: 대규모 트랜잭션 테이블에서 최근 30일 간의 주문 데이터를 자주 조회해야 할 때
  - **해결책**: 최근 데이터만 인덱싱하여 성능을 최적화하기 위해 Partial Index를 사용
  - 사용 이유: 전체 테이블을 인덱싱하는 대신, 자주 조회되는 최근 데이터만 인덱싱함으로써 인덱스 크기를 줄이고 조회 속도를 향상
  - 사용 예

```sql
CREATE INDEX idx_recent_orders
ON orders (order_date)
WHERE order_date > NOW() - INTERVAL '30 days';
```

- 사례 2: 특정 상태의 데이터 필터링
  - **상황**: 사용자 테이블에서 활성화된 사용자(active=true)만 자주 조회
  - **해결책**: 활성 사용자만 인덱싱
  - 사용 이유 : 활성 사용자만 인덱싱하여 불필요한 인덱싱 작업을 줄이고 조회 성능을 향상
  - 사용 예

```sql
CREATE INDEX idx_active_users
ON users (last_login)
WHERE active = true;
```

### 4-2. BRIN Indx

- 사례 1: 대규모 로그 테이블 최적화

  - **상황**: 수억 개의 행을 가진 로그 테이블에서 시간 범위로 데이터를 자주 조회
  - **해결책**: 시간 컬럼에 BRIN 인덱스를 사용
  - 사용 이유 :  BRIN 인덱스는 범위 기반 인덱싱을 통해 대규모 테이블에서 특정 시간 범위의 데이터를 효율적으로 조회, 디스크 공간을 절약하고 인덱스 생성 및 유지 관리 비용 감소
  - 사용 예

  ```sql
  CREATE INDEX idx_log_time
  ON logs USING BRIN (log_time);
  ```

- 사례 2: 지리적 데이터 최적화 (특정한 경우 사용)

  - **상황**: 대규모 지리적 데이터 테이블에서 특정 지역 범위의 데이터를 자주 조회
  - **해결책**: 지리적 좌표에 BRIN 인덱스를 사용
  - 유의점 : 공간 데이터에 대한 쿼리에서 효율적이지 못할 수 있지만, 특정 조건하에서 유용
    - 그러나 대부분 특정 공간 인덱싱에 특화된 인덱스를 사용 (**GiST** ,**SP-GiST, R-Tree)**
  - **사용 이유**: BRIN 인덱스는 지리적 좌표 범위를 효율적으로 인덱싱하여 대규모 지리적 데이터의 조회 성능을 최적화
  - 사용 예

  ```sql
  CREATE INDEX idx_geo_location
  ON geo_data USING BRIN (latitude, longitude);
  ```

### 추후 추가 할 점

- 인덱스 튜닝 사례 (직접 인덱스를 튜닝해보면서 사례 작성)
  - ex) nshop
