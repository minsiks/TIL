# 24-04-26 AgensSQL Day 11

## AgensSQL 

### 1.Database, Tablespace, User, schema

- Postgres의 Database 구조

![image-20240426135141539](C:\Users\min\Documents\TIL\assets\image-20240426135141539.png)

​	Postgres의 모든 Database는 객체식별자(OID)로 관리 (부호 없는 4Byte 범위)

​	DB 생성시 OID로 이름을 갖는 디렉토리 및 파일 구조가 생성

​	FSM(Free Space Map) : 쓰기 작업에 사용될 데이터베이스의 빈공간을 관리하기 위한 자료구조 -> 쓰기 작업 성능 향상이 목적

​	VM(Visibility Map) : 테이블 블록에 대한 가시성 기록

- Tablesapce

  - 물리적 계층과 논리적 계층 사이에 추상화 계층을 형성하여 더 나은 논리적 데이터를 제공하는 논리 파일
  - Oracle은 데이터를 테이블스페이스에 논리적으로 저장, 해당 테이블스페이스와 연결된 데이터 파일에 물리적으로 저장
  - 데이터 베이스 개체를 나타내는 파일을 저장하는 위치를 정희 할 수 있음

- User

  - 데이터베이스를 이용하는 모든 상용자

  - postgres에서 user

    - Role은 User와 Group의 개념을 포함

    - Postgres 8.1이전에는 User와 Group이 별개의 entity이지만, 지금은 Role만 존재

    - Role은 'User나 Group, 혹은 둘 다'

    - | 항목        | 요약                                                   |
      | ----------- | ------------------------------------------------------ |
      | SUPERUSER   | 로그인을  제외한 모든 권한검사를 통과                  |
      | CREATEDB    | DB생성  권한                                           |
      | CREATEROLE  | Role  생성                                             |
      | CREATEUSER  | CREATEROLE사용  권장                                   |
      | REPLICAION  | Streaming  Replica 권한                                |
      | INHERIT     | 역할이  권한을 상속함                                  |
      | NOINHERIT   | 역할이  권한을 상속하지 않음(지정하지  않았을 시 기본) |
      | BYPASS  RLS | 모든  행 레벨 보안 정책을 우회                         |
      | LOGIN       | 로그인을  허용(PASSWORD지정  필요)                     |
      | CONNLIMIT   | 동시  연결 제한                                        |
      | VALIDUNTIL  | 역할의  유효 기간    항목                              |

- Schema
  - 자료의 구조, 자료의 표현 방법, 자료 간의 관계를 형식 언어로 정의한 구조
  - 데이터베이스에서 데이터 구조와 그 표현법, 자료 간의 관계를 형식 언어로 정의
  - 대부분의 DBMS에서 schema는 "내부/외부/개념"으로 구분하는 3층 schema구조
    - 외부 스키마
      - 프로그래머나 사용자의 입장에서 데이터베이스의 모습으로 조직의 일부를 정의
    - 개념 스키마
      - 모든 응용 시스템과 사용자들이 필요로 하는 데이터를 통합한 조직 전체의 데이터베이스 구조를 논리적의로 정의
    - 내부 스키마
      - 전체 데이터베이스의 물리적 저장 형태를 기술

- 권한 관리
  - 권한(privilege)
    - 특정 유형의 SQL을 실행하거나 다른 사용자의 Object(DB/Table)에 접근할 수 있는 권한

### 24.04.26 AgensSQL 오전교육

```
Data Access 및 Transaction
> 백엔드 프로세스 & 백그라운드 프로세스 (아키텍쳐에 있음)
>> 백엔드 프로세스 : 쿼리수행이 중요
> Parser : 쿼리 체크
> Simple 쿼리 : 단순 쿼리 (ex. 단일테이블 select )
> Complex : Rewriter 과정 등을 거침
> Statistics : Optimizer 실행을 위해 분석 
> insert 쿼리 수행
>> 쿼리로그 (WAL) 와 Data files 구분해서 저장
커넥션풀

```

#### 데이터 커넥션 풀 (DBCP)

![데이터베이스 커넥션 풀 (Database Connection Pool)](C:\Users\min\Documents\TIL\assets\database-connection-pool.png)

DB 연결할 때마다 Connection 객체를 새로 만드는 것은 비용이 많이 들며, 비효율적

이러한 문제를 해결하기 위해 애플리케이션 로딩 시점에 Connection 객체를 사용, 애플리케이션의 성능을 향상하는 커넥션 풀 등장

- 커넥션 풀 동작 구조
  - 애플리케이션을 시작하는 시점에 커넥션 풀은 필요한 만큼 커넥션을 미리 생성,보관
  - Connection  객체의 개수는 다르지만 일반적으로 기본값으로 10개 생성
  - Connection 객체는 TCP/IP로 DB와 연결되어 있는 상태, 즉시 SQL을 DB에 전달
  - 이미  생성되어 있는 커넥션을 참조하여 사용

### PostgreSql 구조

- PostgreSQL, Oracle비교

  - Oracle 

    - DB엔진이 올라간 후, 인스턴스를 생성 후 database 생성 대부분 1 instance = 1 database
    - 

  - postgresql 

    -  1instance = 1server 의미 서버 단위 개

    ![img](C:\Users\min\Documents\TIL\assets\img.png)
