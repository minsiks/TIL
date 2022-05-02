# 5/2 SQL Day3

```sql
SELECT * FROM cust;
SELECT * FROM item;
SELECT * FROM cart;

INSERT INTO cart
VALUES (NULL,'id03',1000,3,current_date());

DELETE FROM cart WHERE id = 107;

SELECT c.id, cu.name, i.name, i.price, (c.num * i.price)FROM cart c
INNER JOIN cust cu ON c.custid = cu.id
INNER JOIN item i ON c.itemid = i.id
WHERE i.price > 15000;

-- 장바구니에 고객별 총 금액의 평균을 구하시오
SELECT cu.name, AVG(c.num * i.price) AS iavg FROM cart c
INNER JOIN cust cu ON c.custid = cu.id
INNER JOIN item i ON c.itemid = i.id
GROUP BY cu.name
HAVING iavg >= 100000;

-- 사원 정보를 출력 하시오
-- 사원번호, 사원이름, 부서명, 직급명ALTER
SELECT * FROM emp;
SELECT * FROM dept;
SELECT * FROM title;

INSERT INTO emp VALUES ('1011',NULL,NULL,'이말자',NULL,3000,sysdate());
INSERT INTO title VALUES ('50','인턴');

SELECT e.empname, d.deptname, t.titlename FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno;

SELECT d.deptname, AVG(e.salary) FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
INNER JOIN title t ON e.titleno = t.titleno
GROUP BY d.deptname;

-- OUTER JOIN
SELECT * FROM emp e
LEFT OUTER JOIN title t ON e.titleno = t.titleno
UNION
SELECT * FROM emp e
RIGHT OUTER JOIN title t ON e.titleno = t.titleno;

-- 사원 정보를 출력 하시오
-- 이름, 부서명, 직급명 을 출력한다.
-- 단 이말자도 출력한다.
SELECT e.empname, d.deptname, t.titlename FROM emp e
LEFT OUTER JOIN dept d ON e.deptno = d.deptno
LEFT OUTER JOIN title t ON e.titleno = t.titleno;

-- CROSS JOIN
SELECT * FROM emp e
CROSS JOIN title t;
-- SELF JOIN
SELECT * FROM emp;
-- 사원 이름가 매니저 이름을 출력 하시오
-- 단, 모든 직원을 출력 하시오
SELECT e1.empname, e2.empname FROM emp e1
LEFT OUTER JOIN emp e2 ON e1.manager = e2.empno;

-- 직원 정보를 출력 한다.
-- 이름, 부서명, 직급명 ,매니저명
-- 단 모든 직원 정보를 출력 한다.

SELECT e1.empname, d.deptname, t.titlename,e2.empname FROM emp e1
LEFT OUTER JOIN dept d ON e1.deptno = d.deptno
LEFT OUTER JOIN title t ON e1.titleno = t.titleno
LEFT OUTER JOIN emp e2 ON e1.manager = e2.empno;

-- 1. JOIN X
SELECT * FROM emp;
SELECT * FROM title;
SELECT * FROM emp;
-- 1.직원정보를 출력한다. 직원의 연봉 정보와 연봉의 세금 정보를 같이 출력한다 세금은 10%
SELECT empno,titleno,deptno,empname,manager,salary,hdate,(salary*0.1) AS 'TAX' FROM emp;
-- 2. 직원정보 중 2001 이전에 입사하였고 월급이 4000만원 미만인 직원을 조회
SELECT * FROM emp
WHERE YEAR(hdate) < 2001
AND salary < 4000;
-- manager가 있는 직원 중 이름에 '자' 가 들어가 있는 직원정보 조회
SELECT * FROM emp
WHERE manager IS NOT NULL
AND empname LIKE '%자';
-- 4. 월급이 2000미만은 '하' 4000미만은 '중' 4000이상은 '고' 를 출력
SELECT empname,salary,
CASE
	WHEN salary < 2000 THEN '하'
	WHEN salary < 4000 THEN '중'
	WHEN salary >= 4000 THEN '고'
END AS level
FROM emp;
-- 2,3. JOIN O

-- 5. 부서 별 월급의 평균을 구하시오
SELECT d.deptname, AVG(e.salary) FROM emp e
INNER JOIN dept d ON e.deptno= d.deptno
GROUP BY e.deptno
HAVING AVG(e.salary) >= 3000;

-- 6. 부서 별 대리와 사원 연봉의 평균을 구하시오 단, 평균이 2500 이상인 부서만 출력
SELECT d.deptname,t.titlename, AVG(e.salary) FROM emp e
LEFT OUTER JOIN dept d ON e.deptno= d.deptno
LEFT OUTER JOIN title t ON e.titleno = t.titleno
WHERE t.titlename IN ('대리','사원')
GROUP BY e.deptno
HAVING AVG(salary) >= 2500;

-- 7. 2000년 부터 2002년에 입사는 직원들의 월급의 평균을 구하시오
SELECT AVG(salary) as '02-03avg' FROM emp
WHERE year(hdate) BETWEEN 2000 AND 2002;

-- 8. 부서 별 월급의 합의 ranking을 1위부터 조회 하시오
SELECT d.deptname, SUM(e.salary) FROM emp e
INNER JOIN dept d ON e.deptno= d.deptno
GROUP BY e.deptno
ORDER BY SUM(e.salary) DESC;

-- 9. 서울에서 근무하는 직원들을 조회 하시오
SELECT E.empname, d.deptname,t.titlename, d.deptloc FROM emp e
LEFT OUTER JOIN dept d ON e.deptno= d.deptno
LEFT OUTER JOIN title t ON e.titleno = t.titleno
WHERE d.deptloc = '서울';

-- 10. 이영자가 속한 부서의 직원들을 조회 하시오
SELECT e.empname, d.deptname FROM emp e
INNER JOIN dept d ON e.deptno= d.deptno
WHERE d.deptname = (SELECT d.deptname FROM emp e
INNER JOIN dept d ON e.deptno= d.deptno
WHERE e.empname = '이영자');

-- 11. 김강국의 타이틀과 같은 직원들을 조회 하시오
SELECT e.empname,t.titlename FROM emp e
INNER JOIN title t ON e.titleno = t.titleno
WHERE t.titlename = (SELECT t.titlename FROM emp e
INNER JOIN title t ON e.titleno= t.titleno
WHERE e.empname = '김강국');

-- 1. 2000 년 이후 입사 한 사원들의 정보르 출력
SELECT e.empno,e.empname, d.deptname,t.titlename, d.deptloc FROM emp e
INNER JOIN dept d ON e.deptno= d.deptno
INNER JOIN title t ON e.titleno = t.titleno
WHERE year(e.hdate) >= 2000;

-- 2. 부서 이름 별 월급의 평균을 구하시오 단, 평균이 3000 이상인 부서만 출력
SELECT d.deptname, AVG(e.salary) FROM emp e
INNER JOIN dept d ON e.deptno= d.deptno
GROUP BY e.deptno
HAVING AVG(e.salary) >= 3000;
```

