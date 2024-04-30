# 24-04-30 AgensSQL Day 13

## AgensSQL 

### 1.Explain 쿼리

- Scan 유형

  - Sequential Scan (Seq Scan)

    - 테이블의 모든 데이터를 하나씩 확인
    - 주로 인덱스가 없는 column을 조건으로 검색할 경우 사용
    - 순서대로 테이블의 모든행을 읽음(Table scan 혹은 Full scan)

  - index Scan

    - 인덱스를 사용 (* Primary Key에 대해 인덱스를 자동으로 생성)
    - 인덱스가 만들어진 column을 조건문으로 조회할 때 사용
    - 인덱스는 항상 정렬, 역순으로 조회 가능
    - index Only Scan
      - 인덱스에 필요한 데이터가 있는경우 사용
      - 인덱스가 필요한 값이 있기 때문, 실제 데이터를 fetch하지 않아도 됨
      - 인덱스가 "shared_buffer"에 캐시되기 때문에 I/O 및 시간 측면에서 빠름

  - Bitmap Scan

    - Index Scan과 Sequential Scan이 조합

    1. Bitmap Index Scan (단계)
       - 인덱스 자료 구조를 scan, 조건에 해당하는 인덱스의 데이터를 이용 bitmap 생성
       - 인덱스를 탐색하면서 조건에 맞는 자료는 1, 아닌 것은 0. 페이지 단위 비트맵 생성
    2. Bitmap Heap Scan(단계)
       - 생성된 bitmap을 scan하면서 조건에 맞는 실제 데이터를 가져옴

  - EXPLAIN의 output에서는 계획 트리의 각 노드에 대해 하나의 line이 있으며, 기본 노드 유형과 해당 계획 노드 실행을 위해 만든 비용 추정치를 보여줌

    - 노드의 추가 속성을 표시하기 위해, 노드의 요약줄에서 들여쓰기 된 추가 줄 존재
    - 첫 번째 줄(최상위 노드에 대한 요약줄)에는 계획에 대한 '예상 총 실행 비용'이 노출

    ```
    EXPLAIN
    SELECT * FROM bench_2 b;
    QUERY PLAN
    -------------------------------------------------------------
    Bitmap Scan
    Query Explain 5
     Seq Scan on bench_2 b (cost=0.00..38.00 rows=1000 width=189)
    /*
    - cost 왼쪽 행 (Startup Cost) : 측정된 시작 비용으로, 출력 단계가 시작되
     => 시작 비용이 낮을수록 쿼리가 더 빠르게 시작
    - cost 오른쪽 행 (Total Cost) : 측정된 총 비용으로, 계획 노드가 완료될 때
     => 총 비용이 낮을수록 쿼리 실해잉 더 효율적
    - rows : 계획 노드에서 출력(반환)되는 예상 행 수
     => 반환되는 행의 수가 많을수록 처리해야 할 데이터가 더 많아짐
    - width : 계획 노드에서 출력되는 행의 예상 평균 너비(byte)
     => 평균 너비가 작을수록 더 적은 메모리를 사용하여 쿼리를 실행할 수 있음
    */
    ```

    ```
    EXPLAIN
    SELECT
    *
    FROM
      test,
      people
    WHERE 1=1
    AND test.a < 10
    AND test.a = people.id
    ORDER BY
      test.b;
                                         QUERY PLAN
    ------------------------------------------------------------------------------------
     Sort  (cost=19484.70..19484.71 rows=1 width=124)
       Sort Key: test.b
       ->  Gather  (cost=1001.09..19484.69 rows=1 width=124)
             Workers Planned: 2
             ->  Hash Join  (cost=1.09..18484.59 rows=1 width=124)
                   Hash Cond: (test.a = people.id)
                   ->  Parallel Seq Scan on test  (cost=0.00..18483.33 rows=42 width=8)
                         Filter: (a < 10)
                   ->  Hash  (cost=1.04..1.04 rows=4 width=116)
                         ->  Seq Scan on people  (cost=0.00..1.04 rows=4 width=116)
    (10 rows)
    /*
    - Gather : 여러 병렬 작업자의 결과를 단일 결과 집합으로 결합하는 데 사용되는
    */
    ```

  - EXPLAIN ANAYZE

    - planner의 추정치의 정확성을 확인
    - 해당 EXPLAIN은 실제로 쿼리를 실행한 다음 일반 EXPLAIN이 표시하는 것과 동일한 추정치와 함께 각 계획 노드 내에 누적된 실제 행 수와 실제 런타임을 표

### 2. nshop 마이그레이션 (mySQL -> PostgreSQL)

> 참조 url https://csg1353.tistory.com/m/210

user nshop_user01 pwd: nshop1234

- 마이그레이션 과정
  1. postgreSQL(AgensSQL) 설치
  2. 빈 Database 생성 (nshop)
  3. 유저(Role) 생성 및 권한 생성 (nshop_user01 / nshop1234)
  4. database의 권한 생성한 role에 부여
  5. 워크벤치에 data, function, view 등 export
  6. dump.sql 파일 옮긴후 실행
     - 문제 : Backtic(`)로 되어 있어 Syntax오류 표출
     - 해결 : Backtic(`)을 Single quot(')로 변경
  7. pgloader 설치
     - https://ingnoh.tistory.com/160
     - sbcl도 설치
     - https://pgloader.readthedocs.io/en/latest/install.html
     - development Tools 설치요
