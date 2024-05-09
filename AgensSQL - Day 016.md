# 24-05-09 AgensSQL Day 16

## AgensSQL 

### AgesnSQL 24-05-09 오전 교육

```
Sharding
> 노드를 나누어 데이터를 분산
Index
> 컬럼을 Index로 지정하여 정렬하여 블록에 저장
> 인덱스가 많아지면 Index블록이 스플릿되어서 성능이 저하될 가능성이 있음
>> 재정렬이 필요할 수 있음 (delete가 많을때도 동일)
> 쿼리 실행시 테이블의 메타데이터의저장되어있는 정보를 통해 계획을잡아 실행
> 기본적으로 btree Index
> 복합 Index <-> 싱글 Index
>> 중복된 데이터가 많은 컬럼을 index설정시 복합 인덱스를 사용하면 효과적
>> 범위와같은 부분을 복합인덱스로 설정
>> 첫번째 index의 카디널리티가 두번째 index 보다 낮아야 효과
>> 인덱스의 순서를 정할 수?
>> 필수적으로 인덱스를 설정한 컬럼을 검색조건으로 쿼리를 구성
Parallel 
> 오라클 Standard X, Enterprise O
> 단일 파일은 parallel 동작 x
> 파티셔닝을 사용하는 이유 중 데이터 분할도 있지만 Parallel을 동작시키기 위함도 존재
> 메모리 <-> 디스크 단에서 동작 여부 결정 필요
Partition
> 일반적으로 날짜를 이용하여 파티셔닝
> Vertical {Artitioning = 테이블 분할
> 파티셔닝된 테이블 물리적 저장 위치?
>

```

### 1. Partition

- 개념

  - 논리적인 데이터  element들을 다수의 entity로 쪼개는 행위

  - 작은 단위로 물리적으로 분할하는것을 의미

    
