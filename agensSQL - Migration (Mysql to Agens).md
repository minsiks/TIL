# AgensSQL 교육 Review

### 교육 전 이해

- 시스템 엔지니어링 지식 전무
- OS 관련 이해 X
- 데이터(DB) 관련 국비 교육 및 SQLD 과정 중 공부 유일
- Query를 기능에 맞게 사용하는 것에 중점 (효율적인 활용 및 자원 및 비용 고려 x)
- 데이터베이스 구조 및 아키텍쳐 관련 지식
- 기본적인 IT 용어 지식 부족

## 개인 실습. nShop mySQL → PostgreSQL Migratio

---

<aside>
💡 mysql_fdw 설치사례

</aside>

### 1. 부여 받은 시스템 환경에서 Database 생성 (agensSQL설치됨)

### 2. FDW 이해 및 설치

- FDW (Foreign Data Wrapper)
    - PostgreSQL 데이터베이스에서 외부 데이터 소스에 대한 연결을 가능하게 하는 기능
    - PostgreSQL이 다른 데이터 소스를 마치 자체적인 테이블처럼 쿼리하고 조작 가능
    - PostgreSQL 데이터베이스에서 외부 데이터베이스, 파일 시스템, 웹 서비스 등에 대한 연결을 설정
    
    ![0_0v3IILOMgucW1Fzk.webp](AgensSQL%20%E1%84%80%E1%85%AD%E1%84%8B%E1%85%B2%E1%86%A8%20Review%20495450622b7d456682ecc20baf3ff578/0_0v3IILOMgucW1Fzk.webp)
    

### **1. PostgreSQL 14 저장소 추가**

Rocky Linux 8에서 PostgreSQL 14 저장소를 추가하기 위해 다음 URL을 사용합니다:

```jsx
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm
MySQL 개발 라이브러리와 빌드 도구를 설치합니다:
sudo dnf install -y gcc git mysql-devel
mysql_fdw 소스코드를 다운로드하여 빌드합니다:
git clone https://github.com/EnterpriseDB/mysql_fdw.git
```

### 2. 문제 발생

```sql
CREATE EXTENSION mysql_fdw;
경로를 못찾는 문제 발생
agensdb=# CREATE EXTENSION mysql_fdw;
ERROR:  could not open extension control file "/usr/agens/ags-14/share/postgresql/extension/mysql_fdw.control": No such file or directory

```

---

<aside>
💡 pgloader 사용 사례

</aside>

### 1. pgloader 설치

```sql
sudo yum install pgloader
설치 성공
pgloader --help
pgloader --version

명령어 사용하여 설치 확인
```

### 2. pgloader를 사용하여 마이그레이션

[PGLoader](https://access.crunchydata.com/documentation/pgloader/3.6.3/pgloader-usage-examples/)

```c
# Migrating from Mysql Pgloader 문서 사용 예시
createdb pagila
pgloader mysql://user@localhost/sakila postgresql:///pagila]

정리
pgloader mysql://user:password@localhost/source_db_name postgresql:///target_db_name
pgloader mysql://[사용자명]:[비밀번호]@[호스트명]/[소스_데이터베이스_이름] postgresql:///[타겟_데이터베이스_이름]

# 타겟 데이터베이스 생성
CREATE DATABASE nshop;

# 소스데이터베이스 연결 및 migration 과정 중 문제

## 비밀번호 ''없이 인식 x
[agens@localhost:/var/lib/agens]$ pgloader mysql://nshop:!ncomz1234@nshopdb.mysql.database.azure.com/nshop postgresql:///nshop
-bash: !ncomz1234@nshopdb.mysql.database.azure.com/nshop: event not found

## postgreSQL 기본 포트 5333으로 설정 적용
[agens@localhost:/var/lib/agens]$ pgloader mysql://nshop:'!ncomz1234'@nshopdb.mysql.database.azure.com/nshop postgresql:///nshop
DB-CONNECTION-ERROR: Failed to connect to pgsql at :UNIX (port 5432) as user "agens": Database error: Socket error in "connect": 2 (No such file or directory)

# 정상 Migration

[agens@localhost:/var/lib/agens]$ pgloader mysql://nshop:'!ncomz1234'@nshopdb.mysql.database.azure.com/nshop postgresql://localhost:5333/nshop
2024-05-20T05:42:28.023000+01:00 LOG pgloader version "3.6.7~devel"
2024-05-20T05:42:28.147003+01:00 LOG Migrating from #<MYSQL-CONNECTION mysql://nshop@nshopdb.mysql.database.azure.com:3306/nshop {1005AC2F53}>
2024-05-20T05:42:28.147003+01:00 LOG Migrating into #<PGSQL-CONNECTION pgsql://agens@localhost:5333/nshop {1005C8AE73}>
2024-05-20T05:42:40.475288+01:00 WARNING PostgreSQL warning: identifier "idx_57532_fk_t_product_category_fee_ref_to_t_product_category_fee_hist" will be truncated to "idx_57532_fk_t_product_category_fee_ref_to_t_product_category_f"
2024-05-20T05:42:41.965321+01:00 LOG report summary reset
                        table name     errors       rows      bytes      total time
----------------------------------  ---------  ---------  ---------  --------------
                   fetch meta data          0        146                     4.436s
                    Create Schemas          0          0                     0.009s
                  Create SQL Types          0          1                     0.040s
                     Create tables          0        122                     0.498s
                    Set Table OIDs          0         61                     0.017s
----------------------------------  ---------  ---------  ---------  --------------
                nshop.t_login_hist          0      11828   593.7 kB          0.265s
  nshop.batch_job_execution_params          0       1248    78.2 kB          0.175s
nshop.batch_step_execution_context          0       1191   229.2 kB          0.190s
        nshop.batch_step_execution          0       1191   164.5 kB          0.162s
 nshop.t_product_category_language          0        561    13.2 kB          0.146s
         nshop.t_product_view_hist          0       1371    56.8 kB          0.167s
          nshop.batch_job_instance          0        398    20.5 kB          0.100s
      nshop.t_common_code_language          0        412    11.6 kB          0.114s
         nshop.batch_job_execution          0        398    54.9 kB          0.110s
 nshop.batch_job_execution_context          0        398    14.7 kB          0.144s
                nshop.t_order_hist          0        351     8.8 kB          0.091s
               nshop.t_coupon_hist          0        319    32.7 kB          0.144s
                 nshop.t_file_info          0        386   119.0 MB          6.019s
               nshop.t_user_coupon          0        204    10.9 kB          0.120s
          nshop.t_product_category          0        146     4.0 kB          0.162s
  nshop.t_product_category_fee_ref          0        142     4.1 kB          0.123s
                 nshop.t_dlvy_info          0        117     4.6 kB          0.157s
         nshop.t_product_order_ref          0        103    12.5 kB          0.190s
                      nshop.t_user          0         82    18.5 kB          0.151s
               nshop.t_calcul_info          0         63     5.4 kB          0.073s
      nshop.t_product_category_ref          0         53     0.4 kB          0.086s
                nshop.t_anti_order          0         52     1.5 kB          0.088s
              nshop.t_common_code2          0         39     2.4 kB          0.058s
            nshop.t_store_category          0         23     0.5 kB          0.092s
            nshop.t_product_option          0         17     0.3 kB          0.082s
               nshop.t_user_system          0         11     0.7 kB          0.091s
                     nshop.t_store          0         11     1.3 kB          5.104s
            nshop.t_batch_oper_hst          0        312    50.1 kB          0.121s
                    nshop.t_coupon          0        205    20.3 kB          0.086s
             nshop.t_menu_language          0        157     3.0 kB          0.059s
 nshop.t_product_category_fee_hist          0        142     5.0 kB          0.071s
               nshop.t_common_code          0        121     7.6 kB          0.066s
               nshop.t_calcul_hist          0         99     3.9 kB          0.059s
             nshop.t_store_fee_ref          0         10     0.3 kB          0.073s
           nshop.t_user_group_auth          0         80     0.5 kB          0.054s
                nshop.t_sttlm_clct          0          8     1.7 kB          0.065s
                nshop.t_order_info          0         83    14.4 kB          0.068s
                nshop.t_user_group          0          4     0.1 kB          0.078s
          nshop.t_wish_product_ref          0         57     1.5 kB          0.058s
                     nshop.t_batch          0          4     6.8 kB          0.070s
                      nshop.t_menu          0         55     3.4 kB          0.091s
            nshop.t_store_rev_rate          0          4     0.3 kB          0.091s
   nshop.t_product_option_language          0         51     0.5 kB          0.054s
     nshop.batch_job_execution_seq          0          1     0.0 kB          0.082s
                   nshop.t_product          0         31    90.6 kB          0.122s
    nshop.batch_step_execution_seq          0          1     0.0 kB          0.061s
                nshop.t_batch_rule          0          1     0.8 kB          0.066s
 nshop.t_product_order_option_hist          0         20     0.9 kB          0.074s
      nshop.t_product_language_ref          0          0                     0.092s
                   nshop.yj_notice          0         12     1.3 kB          0.058s
        nshop.t_product_option_ref          0         10     0.1 kB          0.064s
            nshop.t_store_fee_hist          0         10     0.4 kB          0.050s
                nshop.t_batch_qury          0         11     5.5 kB          0.057s
                nshop.tb_sd_notice          0          8     0.3 kB          0.065s
                    nshop.t_banner          0          4     0.3 kB          0.057s
            nshop.t_batch_oper_typ          0          4     0.3 kB          0.055s
                     nshop.t_popup          0          3     0.2 kB          0.056s
               nshop.batch_job_seq          0          1     0.0 kB          0.054s
                  nshop.tb_sd_file          0          1     0.1 kB          0.050s
                 nshop.t_batch_seq          0          0                     0.053s
                  nshop.yj_manager          0          1     0.0 kB          0.061s
----------------------------------  ---------  ---------  ---------  --------------
           COPY Threads Completion          0          4                     7.891s
                    Create Indexes          0         73                     1.425s
            Index Build Completion          0         73                     0.036s
                   Reset Sequences          0         12                     0.088s
                      Primary Keys          0         55                     0.138s
               Create Foreign Keys          0         12                     0.041s
                   Create Triggers          0          0                     0.001s
                   Set Search Path          0          1                     0.003s
                  Install Comments          0        388                     0.053s
----------------------------------  ---------  ---------  ---------  --------------
                 Total import time          ✓      22626   120.6 MB          9.676s

```

### 3. Database 데이터 확인

```sql

# nshop database 접근
[agens@localhost:/var/lib/agens]$ asql -d nshop

nshop=# \dt+
                                                         List of relations
 Schema |             Name             | Type  | Owner | Persistence | Access method |    Size    |           Description
--------+------------------------------+-------+-------+-------------+---------------+------------+---------------------------------
 nshop  | batch_job_execution          | table | agens | permanent   | heap          | 80 kB      |
 nshop  | batch_job_execution_context  | table | agens | permanent   | heap          | 64 kB      |
 nshop  | batch_job_execution_params   | table | agens | permanent   | heap          | 152 kB     |
 nshop  | batch_job_execution_seq      | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | batch_job_instance           | table | agens | permanent   | heap          | 64 kB      |
 nshop  | batch_job_seq                | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | batch_step_execution         | table | agens | permanent   | heap          | 256 kB     |
 nshop  | batch_step_execution_context | table | agens | permanent   | heap          | 320 kB     |
 nshop  | batch_step_execution_seq     | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | t_anti_order                 | table | agens | permanent   | heap          | 16 kB      | ANTI 주문 변경
 nshop  | t_banner                     | table | agens | permanent   | heap          | 16 kB      |
 nshop  | t_batch                      | table | agens | permanent   | heap          | 16 kB      | 배치
 nshop  | t_batch_oper_hst             | table | agens | permanent   | heap          | 88 kB      | 배치작업이력
 nshop  | t_batch_oper_typ             | table | agens | permanent   | heap          | 8192 bytes | 정산작업유형
 nshop  | t_batch_qury                 | table | agens | permanent   | heap          | 16 kB      | 배치쿼리
 nshop  | t_batch_rule                 | table | agens | permanent   | heap          | 16 kB      | 배치규칙
 nshop  | t_batch_seq                  | table | agens | permanent   | heap          | 0 bytes    | 배치작업순서
 nshop  | t_calcul_hist                | table | agens | permanent   | heap          | 16 kB      | 정산 이력
 nshop  | t_calcul_info                | table | agens | permanent   | heap          | 8192 bytes | 정산정보
 nshop  | t_common_code                | table | agens | permanent   | heap          | 48 kB      | 공통코드
 nshop  | t_common_code2               | table | agens | permanent   | heap          | 16 kB      | 공통코드
 nshop  | t_common_code_language       | table | agens | permanent   | heap          | 64 kB      |
 nshop  | t_coupon                     | table | agens | permanent   | heap          | 72 kB      | 쿠폰 테이블
 nshop  | t_coupon_hist                | table | agens | permanent   | heap          | 88 kB      | 쿠폰이력 테이블
 nshop  | t_dlvy_info                  | table | agens | permanent   | heap          | 16 kB      | 배송 정보
 nshop  | t_file_info                  | table | agens | permanent   | heap          | 62 MB      | 파일 정보
 nshop  | t_login_hist                 | table | agens | permanent   | heap          | 1024 kB    | 로그인이력정보
 nshop  | t_menu                       | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | t_menu_language              | table | agens | permanent   | heap          | 40 kB      |
 nshop  | t_order_hist                 | table | agens | permanent   | heap          | 56 kB      | 주문 이력
 nshop  | t_order_info                 | table | agens | permanent   | heap          | 56 kB      | 주문정보
 nshop  | t_popup                      | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | t_product                    | table | agens | permanent   | heap          | 112 kB     |
 nshop  | t_product_category           | table | agens | permanent   | heap          | 40 kB      |
 nshop  | t_product_category_fee_hist  | table | agens | permanent   | heap          | 40 kB      | 카테고리 수수료 이력관리 테이블
 nshop  | t_product_category_fee_ref   | table | agens | permanent   | heap          | 40 kB      | 카테고리 수수료 참조 테이블
 nshop  | t_product_category_language  | table | agens | permanent   | heap          | 64 kB      |
 nshop  | t_product_category_ref       | table | agens | permanent   | heap          | 8192 bytes | 상품 카테고리 매핑
 nshop  | t_product_language_ref       | table | agens | permanent   | heap          | 0 bytes    |
 nshop  | t_product_option             | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | t_product_option_language    | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | t_product_option_ref         | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | t_product_order_option_hist  | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | t_product_order_ref          | table | agens | permanent   | heap          | 56 kB      | 상품 주문 연관
 nshop  | t_product_view_hist          | table | agens | permanent   | heap          | 152 kB     | 상품 조회 내역
 nshop  | t_store                      | table | agens | permanent   | heap          | 16 kB      | 상점
 nshop  | t_store_category             | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | t_store_fee_hist             | table | agens | permanent   | heap          | 8192 bytes | 스토어 수수료 이력관리 테이블
 nshop  | t_store_fee_ref              | table | agens | permanent   | heap          | 8192 bytes | 스토어 수수료 참조 테이블
 nshop  | t_store_rev_rate             | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | t_sttlm_clct                 | table | agens | permanent   | heap          | 8192 bytes | 정산 수집 테이블
 nshop  | t_user                       | table | agens | permanent   | heap          | 56 kB      | 사용자정보
 nshop  | t_user_coupon                | table | agens | permanent   | heap          | 48 kB      | 쿠폰사용자 매핑 테이블
 nshop  | t_user_group                 | table | agens | permanent   | heap          | 8192 bytes | 사용자그룹정보
 nshop  | t_user_group_auth            | table | agens | permanent   | heap          | 8192 bytes | 사용자그룹권한정보
 nshop  | t_user_system                | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | t_wish_product_ref           | table | agens | permanent   | heap          | 8192 bytes | 장바구니 상품 연관
 nshop  | tb_sd_file                   | table | agens | permanent   | heap          | 8192 bytes | 공지사항 파일 정보
 nshop  | tb_sd_notice                 | table | agens | permanent   | heap          | 8192 bytes | 공지사항
 nshop  | yj_manager                   | table | agens | permanent   | heap          | 8192 bytes |
 nshop  | yj_notice                    | table | agens | permanent   | heap          | 16 kB      |
(61 rows)

-- 데이터 간 비교
-- mysql
select count(*) from t_login_hist;
11,828
-- agensql
nshop=# select count(*) from t_login_hist;
 count
-------
 11828
(1 row)
```

### 4. dbeaver 및 jdbc 연결 허용 및 ssh 터널링

```sql
nshop=# SELECT inet_server_addr() AS host, inet_server_port() as port;
 host | port
------+------
      |
(1 row)

```

- 변경한 프로젝트 소스

```java
// application.yml

spring:
  datasource:
    hikari:
      driver-class-name: net.sf.log4jdbc.sql.jdbcapi.DriverSpy
      jdbc-url: jdbc:log4jdbc:mysql://nshopdb.mysql.database.azure.com:3306/nshop?characterEncoding=utf-8
      username: nshop
      #password: nshop
      password: "!ncomz1234"
      connection-test-query: SELECT 1
      connection-timeout: 20000
      allow-pool-suspension: true
      minimum-idle: 1
      maximum-pool-size: 4
      
      |
      |
      V
      
      
spring:
  datasource:
      driver-class-name: net.sf.log4jdbc.sql.jdbcapi.DriverSpy
      jdbc-url: jdbc:log4jdbc:postgresql://localhost:5333/nshop
      username: agens
      #password: nshop
      password: "agens"
      connection-test-query: SELECT 1
      connection-timeout: 20000
      allow-pool-suspension: true
      minimum-idle: 1
      maximum-pool-size: 4
  ssh:
    tunnel:
      ssh-host: 192.168.56.1 # Web Server Public IP
      ssh-port: 22
      ssh-username: root
      ssh-password: root
    
    
    
// DatabaseConfig.java
// 히카리 삭제 (ssh 연결 편의를 위해..) 
// SshTunnelingProperties,DataSourceProperties  생성
// ssh session 연결
@Bean
	@ConfigurationProperties(prefix = "spring.datasource")
	public DataSourceProperties dataSourceProperties() {
        return new DataSourceProperties();
    }
	
	@Bean
    @ConfigurationProperties(prefix = "spring.ssh.tunnel")
    public SshTunnelingProperties sshTunnelingProperties() {
            return new SshTunnelingProperties();
    }
    
    @Bean
    public DataSource dataSource(SshTunnelingProperties sshTunnelingProperties, DataSourceProperties dataSourceProperties) throws JSchException {
    	JSch jsch = new JSch();
        Session session = jsch.getSession(sshTunnelingProperties.getSshUsername(), sshTunnelingProperties.getSshHost(), sshTunnelingProperties.getSshPort());
        session.setPassword(sshTunnelingProperties.getSshPassword());
        session.setConfig("StrictHostKeyChecking", "no");
        session.connect();

        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName(dataSourceProperties.getDriverClassName());
        dataSource.setUrl(dataSourceProperties.getJdbcUrl());
        dataSource.setUsername(dataSourceProperties.getUsername());
        dataSource.setPassword(dataSourceProperties.getPassword());

        return dataSource;
    }
    
// pom.xml mysql 커넥터 삭제 후 postgresql 추가 
// ssh 터널 연결을 위해 jsch 추가
		
		/* <dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency> */
		
    <dependency>
	    <groupId>org.postgresql</groupId>
	    <artifactId>postgresql</artifactId>
	    <version>9.4-1200-jdbc41</version>
		</dependency>
		
		<dependency>
		  <groupId>com.jcraft</groupId>
		  <artifactId>jsch</artifactId>
		  <version>0.1.55</version>
		</dependency>

```

### 5. 쿼리 이슈 해결

- ERROR: function ifnull(character varying, character varying) does not exist

```sql
select /* CommonCodeMapper.xml.listCategoryChained */
			C.category_id
			, C.parent_id
			, IFNULL(L.TITLE, C.TITLE) AS title,
			C.expand,
			C.confirm_yn,
			(SELECT COUNT(*) FROM T_PRODUCT_CATEGORY AS Child WHERE Child.parent_id = C.category_id) AS child_count
		from
			T_PRODUCT_CATEGORY C
			LEFT OUTER JOIN T_PRODUCT_CATEGORY_LANGUAGE L ON C.CATEGORY_ID = L.CATEGORY_ID AND L.LANGUAGE = 'ko'
		where
			parent_id = '0'
			and C.confirm_yn = 'Y'
			order by display_order
			
-- coalesce로 변경(Mysql, MsSQL ifnull(a,b)가 postgresql coalesce(a,b)로 쓰임

select /* CommonCodeMapper.xml.listCategoryChained */
			C.category_id
			, C.parent_id
			, COALESCE(L.TITLE, C.TITLE) AS title,
			C.expand,
			C.confirm_yn,
			(SELECT COUNT(*) FROM T_PRODUCT_CATEGORY AS Child WHERE Child.parent_id = C.category_id) AS child_count
		from
			T_PRODUCT_CATEGORY C
			LEFT OUTER JOIN T_PRODUCT_CATEGORY_LANGUAGE L ON C.CATEGORY_ID = L.CATEGORY_ID AND L.LANGUAGE = 'ko'
		where
			parent_id = '0'
			and C.confirm_yn = 'Y'
			order by display_order;
```

- ERROR: operator is not unique: bigint = character varying

```sql
select /* CommonCodeMapper.xml.listCategoryChained */
			C.category_id
			, C.parent_id
			, COALESCE(L.TITLE, C.TITLE) AS title,
			C.expand,
			C.confirm_yn,
			(SELECT COUNT(*) FROM T_PRODUCT_CATEGORY AS Child WHERE Child.parent_id = C.category_id) AS child_count
		from
			T_PRODUCT_CATEGORY C
			LEFT OUTER JOIN T_PRODUCT_CATEGORY_LANGUAGE L ON C.CATEGORY_ID = L.CATEGORY_ID AND L.LANGUAGE = 'ko'
		where
			parent_id = '0'
			and C.confirm_yn = 'Y'
			order by display_order

-- parent_id가 bigInt Type, '0' 문자열과 비교, bigint 형 변환

select /* CommonCodeMapper.xml.listCategoryChained */
			C.category_id
			, C.parent_id
			, COALESCE(L.TITLE, C.TITLE) AS title,
			C.expand,
			C.confirm_yn,
			(SELECT COUNT(*) FROM T_PRODUCT_CATEGORY AS Child WHERE Child.parent_id = C.category_id) AS child_count
		from
			T_PRODUCT_CATEGORY C
			LEFT OUTER JOIN T_PRODUCT_CATEGORY_LANGUAGE L ON C.CATEGORY_ID = L.CATEGORY_ID AND L.LANGUAGE = 'ko'
		where
			parent_id = '0'::bigint
			and C.confirm_yn = 'Y'
			order by display_order
```

- JOIN 컬럼 TYPE 불일치 (operator is not unique: character varying = bigint)

```sql
SELECT       	/* BannerMapper.xml.getMainBannerInfo */
			f.file_id,
			f.org_filename,
			b.banner_url,
			b.banner_bgcolor
		FROM T_FILE_INFO AS f       
		JOIN T_BANNER AS b          
		oN f.key_id= b.banner_id   
		WHERE 1=1
		AND f.file_type = #{fileType}
		AND b.banner_stat_yn = 'Y'
		AND f.temp_yn = 'N'    
		ORDER BY f.file_id DESC

-- bigint로 형변환하여 해결

SELECT       	/* BannerMapper.xml.getMainBannerInfo */
			f.file_id,
			f.org_filename,
			b.banner_url,
			b.banner_bgcolor
		FROM T_FILE_INFO AS f       
		JOIN T_BANNER AS b          
		oN f.key_id::bigint = b.banner_id   
		WHERE 1=1
		AND f.file_type = #{fileType}
		AND b.banner_stat_yn = 'Y'
		AND f.temp_yn = 'N'    
		ORDER BY f.file_id DESC
```

- ERROR: syntax error at or near ":=” (페이징 헤더)

```sql
SELECT * 
		  FROM (
				SELECT @rownum := @rownum + 1 AS rownum, listA.*
				  FROM (SELECT @rownum := 0) r
				     , (

-- @rownum은 mysql 사용 row_number() 로 변경 *over 필수
-- WITH절을 사용할까 했지만 쿼리의 공통 코드로 페이징을 하는 많은 쿼리 상단에 붙도록 되어 있어 형식에 맞춰 수정
-- 해당 코드를 받는 footer 및 페이징 param 및 처리가 되어 있어 최대한 형식 유지
SELECT * 
		  FROM (
SELECT ROW_NUMBER() over (order by (select 1)) as rownum,listA.* from (SELECT 1) r ,(
```

- ERROR: function f_code_name(unknown, character varying, character varying) does not exist
    - **`pg_loader`**는 MySQL 데이터베이스를 PostgreSQL로 마이그레이션하는 데 유용한 도구이지만, 함수와 프로시저를 자동으로 변환하지 않습니다. 대신, 함수와 프로시저를 수동으로 변환해야 합니다.  (부정확)
    - pg_loader로 Migration과정중 Function 누락 수동으로 추가

```sql
SELECT * 
		  FROM (
				SELECT ROW_NUMBER() over (order by (select 1)) as rownum,listA.* from (SELECT 1) r ,(

		SELECT /* front ProductMapper.xml.getProductList */
			P.prod_id,
			prod_name,
			prod_price,
			prod_stat,
			F_CODE_NAME('P001', prod_stat, 'ko') AS prod_stat_name,
			create_user_id,
			(select usr_nm from T_USER where usr_id = create_user_id) as create_user_name,
			DATE_FORMAT(create_datetime, '%Y-%m-%d %H:%i:%s') AS create_datetime,
			(SELECT GROUP_CONCAT(IFNULL(L.TITLE, C.TITLE)) as category_info
		        FROM T_PRODUCT_CATEGORY_REF ref, T_PRODUCT_CATEGORY C
		        LEFT OUTER JOIN T_PRODUCT_CATEGORY_LANGUAGE L ON C.CATEGORY_ID = L.CATEGORY_ID AND L.LANGUAGE = 'ko'
		        WHERE ref.prod_id = P.prod_id and ref.category_id = C.category_id

		        ORDER BY ref.display_order asc
		    ) as category_info,
		    store_id,
		    recommend_yn
		FROM T_PRODUCT P
        WHERE 1=1

			AND prod_stat = '10'

		order by create_datetime desc

								) listA
						) listB
				  WHERE 
					listB.rownum > (1 - 1) * 10 AND listB.rownum <= (1 - 1) * 10 + 10;
					
					
CREATE OR REPLACE FUNCTION nshop.f_code_name(p_grp_cd VARCHAR(20), p_dtl_cd VARCHAR(20), p_lang_cd VARCHAR(5))
RETURNS VARCHAR(4000) LANGUAGE plpgsql AS $$
BEGIN
    RETURN (
        SELECT 
            CASE 
                WHEN COALESCE(l.dtl_nm, '') = '' THEN c.dtl_nm 
                ELSE l.dtl_nm 
            END AS dtl_nm 
        FROM t_common_code c
        LEFT JOIN t_common_code_language l
        ON l.grp_cd = c.grp_cd 
        AND l.depth = c.depth 
        AND l.dtl_cd = c.dtl_cd 
        AND l.language = p_lang_cd
        WHERE c.grp_cd = p_grp_cd
        AND c.depth = 3
        AND c.dtl_cd = p_dtl_cd
    );
END;
$$;

```

- 동일 쿼리 ERROR: function date_format(timestamp with time zone, unknown) does not exist

```sql

```