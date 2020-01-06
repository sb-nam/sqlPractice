# sqlPractice

```sql
--174~175쪽 1번 문제
SELECT EMPNO,RPAD(SUBSTR(EMPNO,1,2),4,'*') AS MASKING_EMPNO,
       ENAME,RPAD(SUBSTR(ENAME,1,1),5,'*') AS MASKING_ENAME
FROM EMP WHERE LENGTH(ENAME) = 5; 

--174~175 2번 문제
SELECT EMPNO, ENAME, SAL,
    TRUNC(SAL/21.5,2) AS DAY_PAY,
    ROUND(SAL/21.5/8,1) AS TIME_PAY
FROM EMP;    

--174~175 3번 문제
SELECT EMPNO,ENAME,HIREDATE,TO_CHAR(ADD_MONTHS(HIREDATE,3),'YYYY/MM/DD') AS R_JOB, 
CASE 
WHEN COMM IS NULL THEN 'N/A' ELSE TO_CHAR(COMM) END AS COMM
FROM EMP;


--174~175 4번 문제
SELECT EMPNO,ENAME,MGR,
CASE 
WHEN MGR IS NULL THEN '0000' 
WHEN SUBSTR(MGR,-LENGTH(MGR),2) = 75 THEN '5555'
WHEN SUBSTR(MGR,-LENGTH(MGR),2) = 76 THEN '6666'
WHEN SUBSTR(MGR,-LENGTH(MGR),2) = 77 THEN '7777'
WHEN SUBSTR(MGR,-LENGTH(MGR),2) = 78 THEN '8888'
ELSE TO_CHAR(MGR) END AS CHG_MRG
FROM EMP;

```

```sql

-- 212~213 1번 문제
select deptno,trunc(avg(sal)) as avg_sal ,max(sal) as max_sal, min(sal) as min_sal ,count(*) as cnt 
from emp group by deptno; 

-- 212~213 2번 문제
select job, count(*) from emp group by job having COUNT(*) >= 3;

-- 212~213 3번 문제
select to_char(hiredate, 'yyyy') as hire_year, deptno, count(*) as cnt from emp
group by to_char(hiredate, 'yyyy'),deptno ;

-- 212~213 4번 문제
select 'x' as exist_comm , COUNT(*) from emp where comm is null
union all
select 'o' as exist_comm, count(*) from emp where comm  >= 0;

-- 212~213 5번 문제
select deptno,to_char(hiredate,'yyyy'),count(*) as cnt, max(sal) as max_sal , sum(sal) as sum_sal , avg(sal) as avg_sal
from emp
group by deptno, to_char(hiredate,'yyyy');

```

```sql

-- 239~ 240 1번 문제
select d.deptno, d.dname, e.empno, e.ename, e.sal
from dept d, emp e 
where e.deptno = d.deptno and sal > 2000;

-- 239~ 240 2번 문제
select d.deptno, d.dname, trunc(avg(sal)) as avg_sal, max(sal) as max_sal, min(sal) as min_sal, count(*) as cnt
from  dept d, emp e 
where e.deptno = d.deptno 
group by d.deptno, d.dname;

-- 239~ 240 3번 문제
select d.deptno, d.dname, e.empno, e.ename, e.job, e.sal
from dept d, emp e 
where e.deptno = d.deptno 
order by e.deptno, d.dname;

```
