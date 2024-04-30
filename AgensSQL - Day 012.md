# 24-04-29 AgensSQL Day 12

## AgensSQL 

### 1.Database, Tablespace, User, schema

### 24.04.29 AgensSQL 오전교육

```
데이터베이스 구조
> 클러스터 = 1 server
> Tablespace : 데이터를 저장하는 장소
> 테이블이 다른스키마에도 존재할 수 있는 예외적인 경우가 특징
> 데이터베이스 오너 권한에대해서 테이블별로 권한을 부여해야 DML을 사용 가능
> 절차적인 FUNCTION은 지양, 한번에 처리하는 쿼리 지향
> FUNCTION은 절차적인 프로세스가 필수 , 그렇기에 FUNCTION 사용 지양
> MERERIALIZED VIEW : 실제 데이터를 가지고 있는 VIEW, 실제 테이블과 데이터 정합성을 위해 싱크 필요
> SYNONYM은 지양 (점점 안쓰는 추세)
>> 객체가 가진 고유한 이름에 대한 동의어를 지정
>> 영속적인 별명을 부여
> PostgreSQL 새로운 테이블이 생성될 때 권한을 부여해야만 한다. (스카마 권한을 따라가지 않음)
> Hypertables : 여러 tablesspace를 걸치는 테이블
>> Hypertables 사용사례 : 2개의 테이블스페이스가 있을경우 오래된(자주 조회하지않은) 데이터는 비교적 저렴한 2번 테이블 스페이스로 저장, 최근 (자주쓰는) 데이터는 성능이좋은 테이블 스페이스로 저장 등.

?인덱스의 종류, JOB, 오브젝트의 타입, EDB, SYNONYM, 1:1로 VIEW와 테이블을 매핑하는 이유, HA
```

### 2. RAID(Redundant Array of Inexpensive Disks)

​	개별 디스크드라이브를 묶어서 고가의 대용량 고성능 디시크 드라이브의 성능과 기술을 구현하기 위해 만들어진 기술

- RAID 종류
  - RAID 0 (Striping)
    - 드라이브를 병렬로 사용, I/O를 여러 드라이브에 나눠(스트라이프해서) 저장
    - 가장 나은 임의 접근 I/O와 순차 I/O 성능을 제

![image-20240429173621779](C:\Users\min\Documents\TIL\assets\image-20240429173621779.png)
