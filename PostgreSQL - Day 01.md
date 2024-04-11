# 24-04-11 PostgreSQL Day 01

## PostgreSQL

### 1. PostgreSQL이란

- PostgreSQL은 북미와 일본에서는 높은 인지도와 많은 인기를 얻고 있는 RDBMS
- 객체-관계형 데이터베이스 시스템(ORDBMS)
- 데이터베이스 계의 거장 Michael Stonebraker가 시작한 프로젝트

### 2. PostgreSQL 기능 및 제한

- 관계형 DBMS의 기본적인 기능인 트랜잭션과 ACID(Atomicity, Consistency, Isolation, Durability) 지원

- 

- | 항목                                         | 제한 사항  |
  | -------------------------------------------- | ---------- |
  | 최대 DB 크기(Database Size)                  | 무제한     |
  | 최대 테이블 크기(Table Size)                 | 32TB       |
  | 최대 레코드 크기(Row Size)                   | 1.6TB      |
  | 최대 컬럼 크기(Field Size)                   | 1 GB       |
  | 테이블당 최대 레코드 개수(Rows per Table)    | 무제한     |
  | 테이블당 최대 컬럼 개수(Columns per Table)   | 250~1600개 |
  | 테이블당 최대 인덱스 개수(Indexes per Table) | 무제한     |

### 3. PostgreSQL 사용이유

- 상용 DBMS(ex. Oracle)의 높은 라이선스 비용, 마이그레이션 복잡성
- 오픈소스 DBMS
- 비용 절감
- 빠르고 유연한 개발
- 호환성/유연성
- 신뢰성/안정성

