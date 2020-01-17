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
```sql
--262~263 no.1
select e.job,e.empno,e.ename,e.sal,e.deptno,d.dname 
from emp E join dept D on(e.deptno = d.deptno)
where job = (select job from emp where ename = 'ALLEN');

--262~263 no.2
select e.empno, e.ename, d.dname, e.hiredate, d.loc, e.sal, s.grade 
from emp E join dept D on (e.deptno = d.deptno)
join salgrade s on (e.sal between s.losal and s.hisal)
where e.sal > (select avg(sal) from emp)
order by e.sal desc,e.empno ;

--262~263 no.3
select e.empno, e.ename, e.job, d.deptno, d.dname, d.loc
from emp E join dept D on (e.deptno = d.deptno)
where e.deptno = 10 and job not in(select job from emp where deptno = 30);
--     부서번호가 10이고  ,   30에 포함된 직업이 아닌 애들

--262~263 no.4
select e.empno, e.ename, e.sal, s.grade
from emp E join salgrade S on(e.sal between s.losal and s.hisal)
where e.sal > all(select sal from emp where job = 'SALESMAN')
order by e.empno;
```

```sql
--287~288
create table chap10hw_emp as select * from emp;
create table chap10hw_dept as select * from dept;
create table chap10hw_salgrade as select * from salgrade;


--287~288 NO.1
insert ALL into chap10hw_dept(deptno, dname, loc) VALUES(50,'ORACLE','BUSAN')
INTO chap10hw_dept VALUES(60,'SQL','ILSAN')
INTO chap10hw_dept VALUES(70,'SELECT','INCHEON')
INTO chap10hw_dept VALUES(80,'DML','BUNDANG')
SELECT * FROM DUAL;

--287~288 NO.2
INSERT ALL INTO chap10hw_emp(EMPNO,ENAME,JOB,MGR,HIREDATE,SAL,COMM,DEPTNO) 
VALUES(7201,'TEST_USER1','MANAGER',7788,TO_DATE('2016-01-02','YYYY-MM-DD'),4500,NULL,50)
INTO chap10hw_emp VALUES(7202,'TEST_USER2','CLERK',7201,TO_DATE('2016-02-21','YYYY-MM-DD'),1800,NULL,50)
INTO chap10hw_emp VALUES(7203,'TEST_USER3','ANALIST',7201,TO_DATE('2016-04-11','YYYY-MM-DD'),3400,NULL,60)
INTO chap10hw_emp VALUES(7204,'TEST_USER4','SALESMAN',7201,TO_DATE('2016-05-31','YYYY-MM-DD'),2700,300,60)
INTO chap10hw_emp VALUES(7205,'TEST_USER5','CLERK',7201,TO_DATE('2016-07-20','YYYY-MM-DD'),2600,NULL,70)
INTO chap10hw_emp VALUES(7206,'TEST_USER6','CLERK',7201,TO_DATE('2016-09-08','YYYY-MM-DD'),2600,NULL,70)
INTO chap10hw_emp VALUES(7207,'TEST_USER7','LECTURER',7201,TO_DATE('2016-10-28','YYYY-MM-DD'),2300,NULL,80)
INTO chap10hw_emp VALUES(7208,'TEST_USER8','STUDENT',7201,TO_DATE('2018-03-09','YYYY-MM-DD'),1200,NULL,80)
SELECT * FROM DUAL;

select * from chap10hw_emp;
select * from chap10hw_dept;
select * from chap10hw_salgrade;

--287~289 no.3
update chap10hw_emp set deptno = 70
where sal > (select avg(sal) from chap10hw_emp where deptno = 50);

--287~289 no.4
update chap10hw_emp set sal = sal*1.1, deptno = 80 where hiredate > (select min(hiredate) 
from chap10hw_emp where deptno = 60);
select * from chap10hw_emp;
select * from chap10hw_dept;
select * from chap10hw_salgrade;

--297~289 no.5
delete from chap10hw_emp where empno in (select e.empno from chap10hw_emp E join salgrade S on
(e.sal between s.losal and s.hisal and s.grade = 5));
select * from chap10hw_emp;

```

```sql
-- ex) no.1
select trunc(avg(sal)), max(sal), min(sal), count(*) from emp where deptno = 10; 

-- ex) no.2
select deptno, job, count(*) from emp group by deptno,job order by deptno;

-- ex) no.3
select trunc(avg(sal)), max(sal), min(sal) from emp group by job order by trunc(avg(sal));

-- ex) no.4
select e.deptno, d.dname, e.ename, e.sal from emp E join dept D on(e.deptno = d.deptno);

-- ex) no.5
select d.dname from dept D join emp E on(d.deptno = e.deptno) where e.ename = 'ALLEN';

-- ex) no.6
select e.ename, d.dname, e.sal, d.loc from emp E join dept D on(e.deptno = d.deptno)
where e.job = (select job from emp where ename = 'ALLEN');

-- ex) no.7
select e.empno, e.ename, d.dname, e.hiredate, d.loc, e.sal from emp E join dept D on(e.deptno = d.deptno)
where e.sal > (select avg(sal) from emp);

-- ex) no.8
select e.empno, e.ename, d.dname, e.hiredate, d.loc from emp E join dept D on(e.deptno = d.deptno)
where e.deptno = 10 and e.job in(select job from emp where deptno = 20);

-- ex no.9
select e.empno, e.ename, d.dname, d.loc, e.sal from emp E join dept D on(e.deptno = d.deptno)
where e.sal BETWEEN (SELECT SAL FROM EMP WHERE ENAME = 'ALLEN') AND (SELECT SAL FROM EMP WHERE ENAME = 'FORD') 
AND e.ename NOT IN ('ALLEN');

--ex) no.10
select empno,ename,sal from emp where sal > (select max(sal) from emp where deptno = 30);

-- ex) no.11
select count(*) from emp where ename like '_A%';

-- ex) no.12
select round(avg(sal)), sum(sal), max(sal), min(sal) from emp;

-- ex) no.13
select job,round(avg(sal)) from emp group by job having avg(sal) >1500;

-- ex) no.14
select e.ename,d.dname,e.sal from emp E join dept D on(e.deptno = d.deptno);

-- ex) no.15
select d.deptno,d.dname,d.loc,count(e.deptno) from  emp E right outer join dept D on(e.deptno = d.deptno) 
group by d.deptno,d.dname,d.loc;

-- ex) no.16
select empno, ename from emp where sal > (select avg(sal) from emp);

-- ex) no.17
select e.ename, d.dname, e.job, e.sal from emp E join dept D on(e.deptno = d.deptno)
where d.dname = (select d.dname from emp E join dept D on(e.deptno = d.deptno) where ename ='FORD');

-- ex) no.18
select e.ename, e.deptno, e.sal, e.job from emp E join dept D on(d.deptno=e.deptno) 
group by e.deptno,e.ename,e.sal, e.job  HAVING avg(sal) between 
(select avg(sal) from emp E join dept D on(d.deptno=e.deptno) where d.dname = 'SALES') and
(select avg(sal) from emp E join dept D on(d.deptno=e.deptno) where d.dname = 'RESEARCH');

-- ex) no.19
select deptno,ename from emp group by deptno,ename having avg(sal) < 1000; 

-- ex) no.20
select empno, ename, job, sal from emp where sal < (select sal from emp where job = 'ANALYST')
and job not in ('ANALYST');
```
```sql

-- 자바 
CREATE TABLE JAVASTUDENT(
STDNO NUMBER(2) PRIMARY KEY,
NAME VARCHAR(10),
TNO NUMBER (2)
);

CREATE TABLE TEAM(
TNO NUMBER(2) PRIMARY KEY,
TNAME VARCHAR(20),
CAPTAIN VARCHAR(10)
);
INSERT ALL INTO javastudent VALUES(01,'강지선',20)
INTO javastudent VALUES(02,'공은경',30)
INTO javastudent VALUES(03,'김용우',40)
INTO javastudent VALUES(04,'김현민',10)
INTO javastudent VALUES(05,'김형근',60)
INTO javastudent VALUES(06,'남설빈',10)
INTO javastudent VALUES(07,'박상준',40)
INTO javastudent VALUES(08,'손영욱',50)
INTO javastudent VALUES(09,'송나라',20)
INTO javastudent VALUES(10,'신민철',40)
INTO javastudent VALUES(11,'심지선',20)
INTO javastudent VALUES(12,'오세훈',60)
INTO javastudent VALUES(13,'오진수',10)
INTO javastudent VALUES(14,'유영빈',50)
INTO javastudent VALUES(15,'이미령',30)
INTO javastudent VALUES(16,'이창혁',10)
INTO javastudent VALUES(17,'장소미',30)
INTO javastudent VALUES(18,'장현욱',60)
INTO javastudent VALUES(19,'주민웅',40)
INTO javastudent VALUES(20,'한충구',50)
SELECT * FROM DUAL;
SELECT * FROM javastudent;

INSERT ALL INTO TEAM VALUES(10,'자바자바','남설빈')
INTO TEAM VALUES(20,'한나라투지선','심지선')
INTO TEAM VALUES(30,'상부상조','이미령')
INTO TEAM VALUES(40,'더휴면32사단','김용우')
INTO TEAM VALUES(50,'공격팀','손영욱')
INTO TEAM VALUES(60,'우리의서울','오세훈')
SELECT * FROM DUAL;
SELECT * FROM TEAM;

COMMIT;

--1번
select j.name from javastudent j join team t on(j.tno = t.tno) where t.tname = (select tname from team where tname = '우리의서울') order by j.name; 
--2번
select j.name from javastudent j join team t on(j.tno = t.tno) where t.captain = (select captain from team where captain = '이미령') order by j.name;

```

```sql
--테이블생성
CREATE TABLE Member(
 m_no varchar2(4) PRIMARY KEY,--회원등록번호
 m_name varchar2(40) NOT NULL, --이름
 m_ssn varchar2(14) NOT NULL, --주민번호
 m_phoneNum varchar2(13), --연락처
 m_registdate DATE DEFAULT sysdate -- 등록날짜
);

CREATE SEQUENCE seq_member_no;

select * from member;
commit;
```
