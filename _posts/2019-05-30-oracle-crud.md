---
title: "Oracle CRUD"
layout: post
category: DataBase
tags: [DataBase, Oracle]
excerpt: "Oracle의 CRUD에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## Oracle CRUD

  > MySQL CRUD : [https://baekjungho.github.io/mysql-crud/](https://baekjungho.github.io/mysql-crud/)

## 초기 설정

  - 관리자로 접속 : sqlplus / as sysdba
  - 사용자로 접속 : sqlplus 아이디/비밀번호
  - 사용자 생성 : create user 아이디 identified by 비밀번호;
  - 권한 부여 : grant connect, resource, dba to 아이디;
  - 접속 유저 확인 : show user;
  - SQL에서 빠져나오기 : host
    - host후에 exit입력하면 다시 SQL로 돌아옵니다.

  - 패스워드 변경 방법
    - 관리자 권한으로 접속 : sqlplus / as sysdba
    - 명령어 입력 : alter user lbi5320 identified by 비밀번호

  - hr unlock 후 비밀번호 설정
    - alter user hr identified by 1234 account unlock;

## CRUD

  - 테이블 목록 보기 : select * from tab;
  - 테이블 구조 보기 : desc 테이블명;

  > 외부에서 sql 파일 가져오기(import) : @ 입력후 파일 드래그(@파일경로)

  - 라인사이즈 조정 : set linesize 180

### nvl

  - nvl(null value) : 널값이 있는 컬럼에 주로 사용
    - nvl(컬럼명, 0) : 컬럼값이 null일 경우 값을 0으로 대체

  ```sql
  select ename, salary, job, dno, nvl(commission, 0), salary+12, salary*12+nvl(commission, 0)
  from employee;
  ```

  - nvl2
    - NVL2(컬럼, A, B) : 컬럼값이 null이 아닐 경우에는 A null일 경우에는 B를 리턴

  ```sql
  select ename, salary, commission, nvl2(commission, salary*12+commission, salary*12) from employee
  ```

### distinct

  - distinct : 중복 제거

  ```sql
  select distinct dno from employee;
  ```

### between

  - between A and B : where절에서 사용하며 A와 B사이의 값을 나타낼 때 사용

  ```sql
  select * from employee where salary between 1000 and 1500;
  select * from employee where salary >= 1000 and salary <= 1500;
  ```

### in

  - in(A, B, C ...) : where절에서 사용하며 A, B, C의 값을 나타낼때 사용하며 OR역할을 한다.

  ```sql
  select * from employee where commission in(300, 500, 1400);
  select * from employee where commission=300 or commission=500 or commission=1400;
  ```

### not, <>

  - not, <> : not을 표현 할 때 사용

  ```sql
  select ename from employee where dno <> 10;
  ```

### like

  - like : like에 %, 언더바 (_ )와 같은 조건을 주어 해당 문자열이 포함된 값을 출력할 때 사용
    - % : 하나 이상의 문자와 일치 하는 것
    - _ : 하나의 문자만 일치

  ```sql
  EX) 사원의 이름에 'M'이 포함된 사원 모두를 출력
  select * from employee where ename like '%M%';

  EX) 이름의 두 번째 글자가 "A"인 사원 검색
  select * from employee where ename like '_A%';

  EX) 이름에 A와 E를 모두 포함하고 있는 사원을 출력
  select * from employee where ename like '%A%' and ename like '%E%';

  EX) 1981년도에 입사한 사원을 출력하시오
  select * from employee where hiredate like '81%';
  ```

### null

  - is null : 해당 컬럼값이 null인 경우
  - is not null : 해당 컬럼값이 null이 아닌 경우

  ```sql
  EX) 커미션이 null인 사원을 출력
  select * from employee where commission is null;
  (null이 아니면 is not null)
  ```

### order by

  - order by 컬럼명 desc : 해당 컬럼값을 기준으로 내림차순으로 정렬
  - order by 컬럼명 asc : 해당 컬럼값을 기준으로 오름차순으로 정렬
    - asc는 default이므로 생략 가능

  ```sql
  EX) 급여가 2000을 넘는 사원의 이름과 급여를 급여가 많은 사원부터 작은 사원 순으로 출력
  select ename, salary from employee where salary >= 2000 order by salary desc;
  ```

## sql*Plus

  > sql*Plus 명령은 Oracle에서만 제공하며 대소문자를 구분하지 않습니다.

  - L(List) : 최근에 사용한 쿼리문 가져오기
  - / : 최근에 실행한 쿼리문 다시 실행
  - R(Run) : 최근에 실행한 쿼리문 실행 하고, 쿼리문도 보여주기
  - ED(Edit) : 최근에 실행한 명령어 편집
  - SAV(Save) : 쿼리문을 파일에 저장할 때 사용(기본 확장자는 .sql)

  ```sql
  // sample.sql 생성
  select * from employee
  save sample

  // sample.sql 파일 열기
  ed sample.sql

  // REPLACE : 이전의 내용을 새로운 내용으로 대체
  select ename from employee
  sava sample REPLACE

  // APPEND : 이전의 내용에 새로운 내용을 추가
  select hiredate from employee
  save sample APPEND
  ```

  - spool / spool off : 화면캡처 시작, 종료

  ```sql
  spool test
  select * from employee;
  select * from department;
  spool off
  ```

  - 컬럼 사이즈 늘리기 : DB에는 영향이 없고 출력물에만 영향을 끼칩니다.

  ```sql
  column dname format a40
  select * from department;
  ```

  - An : 문자 형식 칼럼의 출력 크기를 설정
  - 9 : 숫자 형식 칼럼의 출력 길이를 조정 (ex 8,000)
  - 0 : 지정된 길이 만큼 숫자 앞에 0을 추가 (ex 0,008,000)

  ```sql
  column salary format 0,000,000
  column commission format 9,999,999
  ```

  - set pagesize 크기 : pagesize 조정

  ```sql
  set pagesize 10
  ```

## dual

  오라클에서는 dual이라는 테이블을 제공하고 dual안에 dummy 라는 컬럼을 제공하고 있습니다.
  dual 테이블에는 x라는 데이만 저장하고 있습니다.

  ```sql
  select sysdate from dual;
  ```

## 함수

### 문자 함수

  - UPPER : 대문자
  - LOWER : 소문자
  - INITCAP : 첫 글자만 대문자

  ```sql
  select 'oracle like', upper('oracle like'), lower('oracle like'), initcap('oracle like') from dual;
  ```

### 문자 조작 함수

  > 인덱스는 1번부터 시작

  - concat : 문자열 결합(연결)
  - substr : 문자열 추출
    - substr(대상, 시작 위치, 추출할 개수)
  - instr : 특정 문자의 위치값을 판단
    - instr(대상, 찾을 글자, 시작 위치, 몇번째 발견)

  ```sql
  // concat은 문자열 2개만 결합 할 수 있습니다.
  select concat('i', 'like') from dual;

  // -4는 뒤에서 부터 4칸 앞으로 온다음에 3글자 출력
  select substr('i like you', 3, 4) from dual; // like
  select substr('i like you', -4, 3) from dual; // yo

  EX) 사원의 마지마 이름이 'N'으로 끝나는 사원 검색
  select * from employee where substr(ename, -1, 1)='N';

  EX) 이름의 세 번째 자리가 'R'인 사원을 검색
  select * from employee where instr(ename, 'R', 3, 1)=3;

  // 처음으로 발견되는 a위치 : 3
  select instr('Oracle mania', 'a') from dual;

  // 두 번째로 발견되는 a위치 : 9
  select instr('Oracle mania', 'a', 4) from dual;

  // 4번째 인덱스 이후로 두 번째로 발견되는 a위치 : 12
  select instr('Oracle mania', 'a', 4, 2) from dual;
  ```

### Padding

  - LPAD(Left Padding) : 컬럼이나 문자열을 명시된 자릿수에서 오른쪽에 나타내고, 남은 왼쪽 자리를 특정기호로 채움니다.
  - RPAD(Right Padding) : 오른쪽으로 정렬하고 왼쪽에 특정기호로 채움니다.

  ```sql
  // salary 컬럼을 10칸으로 잡고 왼쪽은 * 기호로 표시
  select LPAD(salry, 10, '*') from employee;

  // salary 컬럼을 10칸으로 잡고 오른쪽은 * 기호로 표시
  select RPAD(salry, 10, '*') from employee;
  ```

### 공백 제거

  - TRIM : 양쪽 공백 제거
  - LTRIM : 왼쪽 공백 제거
  - RTRIM : 오른쪽 공백 제거

  ```sql
  select TRIM('  Oracle Mania  ') from dual;

  // 특정 문자를 기준으로 자르기 : racle Mania
  select TRIM('O', 'Oracle Mania') from dual;
  ```

### 숫자 함수

  - ROUND : 반올림
  - TRUNC : 버림
  - MOD : 나머지 값을 반환

  ```sql
  // 99, 98.77, 100
  select 98.7654, round(98.7654), round(98.7654, 2), round(98.7654, -1) from dual

  EX) 사원 번호가 짝수인 사원만 출력하시오
  select * from employee where mod(dno, 2) = 0;
  ```

### 날짜 함수

  - sysdate : 시스템에 저장된 현재 날짜를 반환
  - MONTHS_BETWEEN : 두 날짜 사이가 몇 개월인지 반환
  - NEXT_DAY : 특정 날짜에서 최초로 도래하는 인자로 받은 요일의 날짜를 반환
  - LAST_DAY : 해당 달의 마지막 날짜를 반환

  ```sql
  // 오늘
  select sysdate from dual;

  // 어제, 내일
  select sysdate-1, sysdate+! from dual;

  EX) 사원들의 근무 일수를 구하시오
  select round(sysdate-hiredate) from employee;

  EX) 특정 날짜에서 월을 기준으로 버림한 날짜 구하기
  select hiredate, trunc(hiredate, 'MONTH') from employee;

  EX) 각 사원들이 근무한 개월 수 구하기
  select ename, sysdate, hiredate, trunc(MONTHS_BETWEEN(SYSDATE, hiredate))
  from employee;

  select sysdate, nex_day(sysdate, '토요일') from dual;

  EX) 입사한 달의 마지막 날 구하기
  select ename, hiredate, last_day(hiredate) from employee;
  ```

### 형 변환 함수

  - TO_CHAR : 날짜형 혹은 숫자형을 문자형으로 변환
    - TO_CHAR(number `|` date, format)
    - format은 작은 따옴표로 묶어서 대소문자를 구분해서 사용

  - format 형식
    - YYYY : 연도 4자리
    - YY : 연도 2자리
    - MM : 월을 숫자로 표현
    - MON : 월을 알파벳으로 표현
    - DAY : 요일 표현
    - DY : 요일을 약어로 표현

  ```sql
  select ename, hiredate, to_char(hiredate, 'YY-MM'), to_char(hiredate, 'YYYY/MM/DD DAY')
  from employee;
  ```

  - TO_DATE : 특정 데이터를 날짜형으로 변환 해주는 함수

  ```sql
  select ename from employee where hiredate=to_date(19810220, 'YYYYMMDD');

  EX) 올해 며칠이 지났는지 출력하시오
  select trunc(sysdate - to_date('2019/01/01', 'YYYY/MM/DD')) from dual;
  ```

  - TO_NUMBER : 특정 데이터를 숫자로 변환 해주는 함수

  ```sql
  select to_number('100,000', '999,999') - to_number('50,000', '999,999') from dual;
  ```

### 시간 관련 함수

  - AM or PM : 오전, 오후
    - A.M or P.M
  - HH or HH12 : 시간(1~12) 표시
  - HH24 : 시간(0~23)
  - MI : 분 표시
  - SS : 초 표현

  ```sql
  select TO_CHAR(sysdate, 'YYYY/MM/DD, HH24:MI:SS') from dual;

  EX) 입사일을 연도 2자리(YY), 월은 숫자(MON)로 표시하고 요일은 약어(DY)로 지정하여 출력하시오.
  select ename, to_char(hiredate, 'YY/MON/DD DY') from employee;
  ```

### 통화 기호 붙이기

  - L : 각 지역별 통화 기호를 앞에 붙여줍니다.

  ```sql
  select ename, TO_CHAR(salary, 'L999,999') from employee;
  ```

### NULL IF 함수

  - NULLIF(exp1, exp2) : 두 표현식을 비교하여 동일한 경우 null 값을 반환하고 동일하지 않으면
  exp1 표현식을 반환

  ```sql
  select nullif('A', 'A'), nullif('A','B') from dual;
  ```

### DECODE 함수

  ```sql
  select ename, dno, decode(dno, 0, 'ACCOUNTING', 20, 'RESEARCH', 30, 'SALES', 40, 'OPERATIONS', 'DEFAULT')
  as dname from employee order by dno;

  EX) decode 함수로 직급에 따라 급여를 인상하도록 하시오. 직급이 'ANALYST'인 사원은 200,
  'SALESMAN'인 사원은 180, 'MANAGER'인 사원은 150, 'CLERK'인 사원은 100을 인상하시오.
  select ename, job, salary, decode(job, 'ANALYST', salary+200, 'SALESMAN', salary+180, 'MANAGER', salary+150, 'CLERK', salary+100)
  as update_salary from employee;
  ```

### case 함수

  ```sql
  case when dno=10 then 'accounting' when dno=20 then 'ddd' else 'default' end as dname
  from employee;
  ```

### 그룹 함수

  > 그룹 함수는 자동적으로 Null 값을 제외 하여 계산합니다.

  - sum : 합계
  - avg : 평균
  - count : 총 개수
  - max : 최댓값
  - min : 최솟값

  ```sql
  select sum(salary) "총액", avg(salary) "평균", max(salary) "최대", min(salary) "최소"
  from employee;

  select sum(commission) "커미션 총액" from employee;

  select count(commission) as "커미션 받은 사원 수" from employee;

  select count(distinct job) as "직업의 개수" from employee;
  ```

  - group by : 특정 컬럼을 기준으로 그룹으로 묶을 때 사용
    - group by절 다음에 하나 이상의 열을 나열하여 그룹을 나누면 그룹 및 하위 그룹으로 결과를 반환

  ```sql
  EX) 소속 부서별 평균 급여를 부서 번호와 함께 출력
  select dno as "부서 번호", avg(salary) as "급여 평균" from employee
  group by dno;

  select dno, job, count(*), sum(salary) from employee
  group by dno, job
  order by dno, job;

  EX) 부서별 급여 총액이 3000 이상인 부서의 번호와 부서별 급여 총액을 구하시오.
  select dno, max(salary) from employee group by dno having max(salary) >= 3000;

  EX) MANAGER를 제외하고 급여 총액이 5000 이상인 직급별 급여 총액 구하기
  select job, sum(salary)
  from employee
  where job <> 'MANAGER'
  group by job
  having sum(salary) >= 5000;

  EX) 각 업무별로 급여 최고액, 최저액, 총액, 평균을 출력 하시오.
  컬럼의 별칭은 위와같이 지정하고 평균은 정수로 반올림하시오.
  select job, max(salary) "최고액", min(salary) "최저액", sum(salary) "총액", round(avg(salary)) "평균"
  from employee
  group by job;

  EX) 각 부서에 대해 부서번호, 사원수, 부서내의 모든 사원의 평균 급여를 출력하시오.
  평균 급여는 소수점 둘 째 자리에서 반올림 하시오.
  select dno "부서번호", count(*) "사원수", round(avg(salary), 2) "평균 급여"
  from employee
  group by dno
  order by round(avg(salary), 2) desc;
  ```
