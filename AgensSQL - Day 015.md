# 24-05-08 AgensSQL Day 15

## AgensSQL 

### AgesnSQL 24-05-08 오전 교육

```
Join 알고리즘
> oltp  - Nested Loop Join (메모리가 사용량 적음) 
> olap  - sort Merge Join, Hash join (메모리 사용량 많음)
> explain에서 확인 가능
> 옵티마이저를 통해 최적화 후 선택
> 힌트를 통해 알고리즘 변경 가능
> 워크메모리 사용
Index
> 인덱스 리빌딩 간혹가다 필요한 경우
> 비트리, 브린

```

### 1. JOIN Algorithms

- Nested Loop Join (NL Join)
  - 2개 이상의 테이블에서 하나의 집합을 기준으로 순차적으로 상대방 Row를 결합하여 원하는 결과를 조합
  - Driving Table로 한 테이블을 선정, where 절에 정의된 검색 조건을 만족하는 데이터들을 걸러낸 후 이값을 가지고 조인 대상 테이블(Driven Table)을 반복적으로 검색하면서 Join 조건을 만족하는 최종 결과값을 추출
    - Driving Table : Join 수행 시 먼저 액세스 되어 Access path를 주도하는 테이블
    - Driven table : Join 수행시 Driving Table이 아닌 테이블
  - 장단점
    - 인덱스에 의한 랜덤 액세스에 기반, 대량의 데이터 처리 적합 X
    - Driviing Table이 데이터가 적거나 where 절 조건으로 row의 숫자를 줄일 수 있는 테이블로 지정
    - Driven Table에는 조인을 위한 적절한 인덱스 생성 필요
    - 선행 테이블의 결과를 통해 후행 테이블을 액세스 할 때 랜덤 I/O가 발생
  - 프로그래밍에서 다중 반복문과 형태가 유사
  - Tuning Point
    - Index 설정
      - Driving Table의 경우 Where 조건에 해당 하는 컬럼에 대해서 Index가 있을경우 Filter 작업 성능 향상
      - Driven Table의 경우 Join 조건 및 Where 조건에 대한 컬럼을 대상으로 Index가 있을경우 Full scan을 방지 가능
    - Driving Table 선정
      - 어떤 테이블이 먼저 액세스 되느냐에 따라서 속도의 차이가 크게 날 수 있음
      - Driving Table 에서 조건을 통해 추출되는 Row수가 적어야 함
      - 전체 데이터가 적을 수록 유리
      - Hint를 통한 Driving Table 유도
- Sort Merge Join
  - 조회의 범위가 많을 때 주로 사용하는 조인 방법
  - 양쪽 테이블을 각각 Access하여 그 결과를 정렬, 정렬한 결과를 차례로 scan, 연결고리 조건으로 Merge
  - 조인 조건 칼럼에 인덱스가 없거나, 출력해야 할 결과 값이 많을때 사용
  - 장단점
    - 연결 고리에 인덱스가 전혀 없는 경우
    - 대용량의 자료를 조인할 때 유리한 경우
    - 조인 조건으로 <.>,<=,>=와 같은 범위 비교 연산자가 사용된 경우
    - 인덱스 사용에 따른 랜덤 액세스의 오버헤드가 많은 경우
  - 동작 방식
- Hash Join
