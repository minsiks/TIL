# 4/29 SQL Day2
## Day 1 복습

- SQL은 데이터베이스 회사마다 다 다르다

## Part 2 _ch 06 - SQL 기본

### GROUP BY절

``` SQL
SELECT * FROM buytbl;

-- 회원별 구매 금액의 평균을 구하시오.
SELECT USERID,ROUND(AVG(price),1) AS pavg FROM buytbl
GROUP BY userID
HAVING pavg > 100
ORDER BY pavg DESC;

SELECT COUNT(DISTINCT(userID)) FROM buytbl; -- DISTINCT 중복제거

-- groupName 별 구매 고객의 수를 구하시오.
SELECT groupName, COUNT(DISTINCT(userID)) FROM buytbl
GROUP BY groupName;

-- usertbl 회원 들의 평균 키보다 큰 회원을 조회하시오
SELECT * FROM usertbl
WHERE height > (SELECT round(AVG(height),1) FROM usertbl);

-- 회원 중 폰을 가지고 있는 회원의 수
SELECT COUNT(mobile1) FROM usertbl;

SELECT userID, groupName, sum(price*amount) AS usum FROM buytbl
GROUP BY userID, groupName
HAVING userID IN ('KBS','BBK')
AND groupName IS NOT NULL
ORDER BY userID;

SELECT userID, groupName, sum(price*amount) AS usum FROM buytbl
where userID in ('KBS','BBK')
GROUP BY userID, groupName
ORDER BY userID;

SELECT num, groupName,SUM(price*amount) FROM buytbl -- ROLLUP 집계함수
GROUP BY groupName,num
WITH ROLLUP;
```

### AUTO_INCREMENT

```sql
DROP TABLE IF EXISTS itemtbl;
CREATE TABLE itemtbl(
	id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL UNIQUE,
    price INT NOT NULL,
    regdate DATE
);
ALTER TABLE itemtbl AUTO_INCREMENT = 1000;
INSERT INTO itemtbl VALUES (NULL,'shirts',15000,sysdate());
INSERT INTO itemtbl VALUES (NULL,'pants1',10000,sysdate());
INSERT INTO itemtbl VALUES (NULL,'pants2',20000,sysdate());
INSERT INTO itemtbl VALUES (NULL,'pants3',30000,sysdate());
INSERT INTO itemtbl VALUES (NULL,'pants4',40000,sysdate());
INSERT INTO itemtbl VALUES (NULL,'pants5',50000,sysdate());
INSERT INTO itemtbl VALUES (NULL,'pants6',60000,sysdate());
INSERT INTO itemtbl VALUES (NULL,'pants7',70000,sysdate());
INSERT INTO itemtbl VALUES (NULL,'pants8',80000,sysdate());
INSERT INTO itemtbl VALUES (NULL,'pants9',90000,sysdate());
INSERT INTO itemtbl VALUES (NULL,'pants10',100000,sysdate());
SELECT * FROM itemtbl;

DROP TABLE IF EXISTS carttbl;
CREATE TABLE carttbl (
	id INT PRIMARY key AUTO_INCREMENT,
	userID CHAR(8) NOT NULL,
    itemID INT NOT NULL,
    qt INT NOT NULL,
    regdate DATE
);

-- BBK, KBS ,JYP

INSERT INTO carttbl VALUES(NULL,'BBK',1000,10,sysdate());
INSERT INTO carttbl VALUES(NULL,'KBS',1001,5,sysdate());
INSERT INTO carttbl VALUES(NULL,'JYP',1004,2,sysdate());
SELECT * FROM carttbl;

DELETE FROM carttbl WHERE userID = 'BBB';
```

### 데이터의 삭제 : DELETE FROM

- DELETE FROM tbl;
  - 트랜잭션 로그를 기록
  - 시간이 오래걸림
- DROP TABLE  tbl;
- TRUNCATE TABLE tbl;
- 

### WITH절

```SQL
WITH temp(userID,total)
AS 
(SELECT userID, sum(price*amount) FROM buytbl
GROUP BY userID)
SELECT total FROM temp;

-- 각 지역 별 가장 키가 큰 키들의 평균을 구하시오
WITH temp(addr,max)
AS
(SELECT addr, max(height) FROM usertbl
GROUP BY addr)
SELECT AVG(max) FROM temp;

SELECT AVG(a.hmax) FROM (
SELECT addr, max(height) as hmax FROM usertbl
GROUP BY addr) a;
```

## Ch07 SQL 고급

### 데이터 형식

- char(5)
  - 고정형
  - abc를 넣었을때 abc__
- varchar(5)
  - 가변형
  - abc를 넣었을때 abc

### 데이터 형식 변환 함수

- CONCAT

```sql 
SELECT concat(prodName,'/', groupName) FROM buytbl;
```

### 내장 함수

- IF 절

```SQL
SELECT userID, price*amount AS tt, IF (price*amount > 500, 'High','Low') FROM buytbl;
```

- IFNULL

```sql
SELECT prodName,IFNULL(groupName,'None') FROM buytbl;SELECT 
SELECT COUNT(IFNULL(groupName,'None')) FROM buytbl;
```

- CASE~WHEN~ELSE~END

```SQL
SELECT userID,amount, 
CASE 
	WHEN amount >= 5 AND amount <2 THEN 'C'
	WHEN amount >= 2 AND amount <4 THEN 'B'
    WHEN amount >= 4 AND amount <6 THEN 'A'
    ELSE 'None'
END AS level
FROM buytbl;
```

- FORMAT

```SQL
SELECT format(123456.123456,4);
```

- 날짜 및 시간 함수

```SQL
SELECT mDate,ADDDATE(mDate, interval 30 DAY),
	SUBDATE(mDate, interval 30 DAY) 
FROM usertbl;
 
SELECT curdate();
SELECT curtime();
SELECT now();
SELECT sysdate();

SELECT YEAR(sysdate());

SELECT YEAR(mDate) FROM usertbl;
SELECT date_format(mDate,'%Y-%m-%d') FROM usertbl;

SELECT mDate,datediff(NOW(),mdate) FROM usertbl; -- DATEDIFF 중요! 날짜 일 수를 구함
SELECT mDate,period_diff(DATE_FORMAT(NOW(),'%Y%m'),DATE_FORMAT(mdate,'%Y%m')) 
FROM usertbl;  -- period_diff 개월수를 구함
```

