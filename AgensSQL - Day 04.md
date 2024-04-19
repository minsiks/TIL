# 24-04-17 AgensSQL Day 04

## AgensSQL 

### 1. SQL 작성법

- ex

  - ``` 
    1. 들여 쓰기는 스페이스 2칸
    2. *는 쓰지 않는다. 반드시 컬럼 이름을 쓰도록 한다.
    −	각 작업 단계 별로 독립성을 지니기 위한 방법으로 이전 단계의 작업에서 컬럼이 추가 되더라도 영향을 받지 않기 위함이다.
    −	물론 사용 중인 컬럼 이름의 변경 및 삭제는 영향을 받는다.
    −	SELECT 뿐만 아니라 CREATE, INSERT 키워드에 모두 적용된다
    3. Join을 위해 FROM절에 2개 이상의 Table이 사용되면 반드시 Alias를 사용하고, SQL,에 사용되는 모든 컬럼 앞에는 반드시 Alias를 사용하여 어느 Table에서 왔는지 명확히 알 수 있도록 한다.
    4.	컬럼에 변형을 가하면 반드시 사용될 컬럼의 이름을 Alias를 오른 쪽에 써 주도록 한다.
    5.	모든 Alias는 최대한 의미가 있도록 한다.
    −	FROM 절 : Table의 Initial의 조합
    −	컬럼 Alias : 최종 사용될 컬럼명
    6.	컬럼 리스트는 한 줄에 하나씩 아래로 나열하고 콤마는 컬럼 오른쪽에 쓴다.
    7.	괄호는 컬럼 리스트 줄 아래 위로 위치하도록 한다.
    8.	키워드는 대문자 : 오라클 명령어 등
    9.	사용자가 생성한 명은 소문자(대문자로 해도 무방) : User명, Table명, 컬럼명 등
    
    
    ALTER SESSION ENABLE PARALLEL DML;
    
    INSERT /*+ APPEND PARALLEL(x,4) */ INTO IDBADM.ODS_RT_INFEE_DTL x (
      YR_MTH_DAY_HR_5MIN_CD,
      BILL_DATA_CL_CD,
      ……
    )
    SELECT
      YR_MTH_DAY_HR_5MIN_CD,
      'BDTINFD04C01' BILL_DATA_CL_CD,
      ……
    SUM(PROC_AMT) PROC_AMT
    FROM (
    SELECT /*+ FULL(PN) PARALLEL(PN,4) FULL(PPII) PARALLEL(PPII,4) */
    ……
      PN.BILL_SYS_CD,
      ……
    NVL(PPII.WORK_GROUP_ID,'ZZZZZZZZZZ') WGP_ID,
      ……
    FROM    IDBA.PAY_NUD PN,
            ODISTG.ODS_RT_PROD_PARTNR_ID_INFO PPII
    WHERE   1=1
    ……
    AND     SUBSTR(PN.MSG_TYP,1,1) = '0'
    )
    GROUP BY
      YR_MTH_DAY_HR_5MIN_CD,
      'BDTINFD04C01',
    ……
      ORG_LD_OP_DTM
    ;
    COMMIT
    ;
    ```

### 2. Naming Rules

> Naming Rules의 주된 목적은 본인이 기억하기 쉽고, 팀 내 또는 작업 간 공유하기 쉽도록 하기 위함

- Directory Naming Rule
  - Directory Naming을 세분화해서 관리한다. (프로젝트 명, 프로젝트 솔루션 명 등)
  - 업무 절차에 따라 숫자를 붙여 관리 (01.mk_DATA 02.upd_DATA)
  - 로그와 스크립트를 분리 (스크립트를 생성한 디렉터리에 별도의 LOG 디렉터리를 생성한 후 모든 로그는 LOG 디렉터리에 저장)
  - dbadmin(=ex Vertica DB admin 디렉터리) 폴더는 가급적 사용을 자제
- 파일 Naming Rule
  - 파일 명 작성 시 쿼리나 키워드 중심으로 대문자를 사용 (make- -> mk_파일명.sh)
  - 한눈에 식별 가능한 명확한 이름을 부여 ( 쿼리 파일 불러와서 수행 -> run_실행파일명.sh)
    - 쿼리는 run_xx.sh(불러와서 수행) 스크립트로 수행되도록 권장
  - 길어도 최대한 상세하게 작성한다. (export_objects -> exp_tb명.sql)
  - 의문이 없도록 명확한 이름을 부여 (데이터 삽입 -> ins_TB명.sql)
- 키워드 요약(mk 또는 make로 사용해도 좋으나 a.sql 등 의미 없는 키워드 사용 자제)
  - make -> mk
  - create -> cr
  - drop -> dr
  - insert -> ins
  - select -> sel
  - update -> upd
  - delete -> del
- run_xx.sh 스크립트 수행 예시
  - 

### 3. aql프롬프트 상에서 vi 사용하기

- asql 수행 후 Prompt 상에서 vi로 파일로 만들고 명령어 문장(SQL, asql 명령어 등)을 작성하여 파일로 수행

  - agensdb=# \! vi a.sql
    a.sql 파일에 명령어 작성
    :wq                        - 저장하고 나오면 asql 프로프트로
    agensdb=# 

    agensdb=# \i a.sql   - 파일 수행

    명령어 문장별로 구분하여 파일로 만들어 일부 수정하고 수행하는 것을 반복

    명령어 문장 마지막에 로깅 파일을 설정하여 항상 결과를 보관하는 습관을 들이자
    \o a.log

    검색기능

    Ctrl+r
    Esc                  - 빠져 나오기

### 4. psql 명령어

```
- DB 접속 

# psql -U [DB사용자계정] [데이터베이스명] 

- Postgre SQL shell 진입시 

# psql [스키마명] 

- DB 데이터베이스 출력 

# \l or \list 

- DB 데이터베이스 선택 

# \c [데이터베이스 명] 

- DB 데이터베이스 생성 

#CREATE DATABASE dbname OWNER ownnerName 

- DB 테이블 출력 

# \dt 

- 테이블 구조 조회 : 오라클의 DESCRIBE TABLE 

 # \d+ 테이블명 

- DB 해당 테이블 정보 출력  

# \d [테이블명]  

- DB 나가기(종료) 

# \q 

- DB 사용자 권한 정보 

# \du 

- DB 출력 변경 

# \x 

- DB 쿼리 결과값 파일 저장(쿼리 결과값을 출력하지 않고 파일에 저장됨) 

# \o [파일경로] 

# [select 문 명령어] 

- DB 실행중인 쿼리 조회 

# SELECT * FROM pg_stat_activity ORDER BY query_start ASC; 
- SQL문 파일을 실행 
  $ psql -U [사용자명] [DB명] < [SQL파일명] 

- 파일로부터 DB로 dump, 또는 DB로부터 파일로 dump 

 # COPY 테이블명 [(컬럼명1, 컬럼명2,...)] FROM '파일명' 
  # COPY 테이블명 [(컬럼명1, 컬럼명2,...)] TO '파일명' 
  구분자를 지정하지 않으면 기본적으로 탭으로 지정됨. 전체 옵션은 아래와 같음. 
  http://www.postgresql.org/docs/9.2/static/sql-copy.html 참고 

- 파일로 dump 

   pg_dump -d [db명]  -U [user] -F t > [파일명.tar] 

-d, --dbname : Backup할 Database 명. 

-h, --host : Database 주소. 

-U, --username : Database 접속 시 User ID 

-F, --format : Backup Format. 필자는 주로 tar 파일로 backup하기 때문에 't'를 사용한다. 

-f, --file : Backup File Name 

-t, --table : 특정 Table만 Backup하려할 때 대상이 되는 Table 명 

-j, --jobs : Backup 시 병렬 처리 여부와 그 정도. 

-v, --verbose : 진행 과정 표시. 

- dump to restore 

  pg_restore -h localhost -U postgres -C -d postgres -F t [dumpFile] 

-d, --dbname : Restore하는 Database 명. 

-h, --host : Database 주소. 

-U, --username : Database 접속 시 User ID 

-F, --format : Restore File의 Format. 

-t, --table : 특정 Table만 Restore하려할 때 대상이 되는 Table 명 

-j, --jobs : Restore 시 병렬 처리 여부와 그 정도. 

-v, --verbose : 진행 과정 표시 

-C, --create : Target DB를 새로 만들면서 Restoration 진행. 

-c, --clean : Restoration 시에 같은 이름의 Database Object가 발견되면 Drop 후에 Create하게 함. 

-O, --no-owner : 원본 DB의 Owner가 복구할 위치에 존재하지 않을 경우 복구 시 다량의 에러가 발생한다. 이를 막기 위해  DB 복구시 OWNER를 명시하지 않고 진행하게 함. 
```



### 4.17 오전 Agens 교육

- HA구성

```sql
hb.conf - 항상 신뢰 시 사용가능 >> 비트나인 자료 >엔지니어 교육용 > poc시나리오 설치
postgre parrallel 기능
디스크나 메모리단에서라는 차이 있음 그것을 알아야

wal_buffer : 로그

postgre conf 사이트에서 사양에 따른 평균값을 제시해주는 것이 존재함
https://pgtune.leopard.in.ua/

dw 데이터 웨어하우스 찾기

systemctl status asg-* 로도 가능

스키마와 데이터베이스 차이

\s 와 grep을 합쳐서 많이 사용

```

