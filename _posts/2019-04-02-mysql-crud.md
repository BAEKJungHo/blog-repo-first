---
title: "MySQL CRUD"
layout: post
category: DataBase
tags: [DataBase, MySQL]
excerpt: "MySQL의 DDL, DML, DCL에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

MySQL의 DDL, DML, DCL에 대해 배워 봅시다.

## CRUD

  [SQL Server 설명서](https://docs.microsoft.com/ko-kr/sql/sql-server/sql-server-technical-documentation?toc=..%2ftoc%2ftoc.json&view=sql-server-2017)

  MySQL의 제약조건과, DDL, DML, DCL에 대해 배워 봅시다.

  테이블 조회와, 테이블 이름 변경은 아래와 같습니다.

  > show tables; // 테이블 목록 보기
  >
  > desc address_book; // 해당 테이블 상세 보기(제약조건, 컬럼명 등)
  >
  > rename table tmp to tmp_table; // 테이블 이름 변경

  데이터베이스 사용방법

  > use DB명

  __CRUD !__

  CRUD : C(CREATE) R(READ) U(UPDATE) D(DELETE)

## 제약조건

  제약조건에는 애트리뷰트 제약조건과, 기본키 제약조건, 참조 무결성 제약조건이 있습니다.

  - __애트리뷰트 제약조건__

    - NOT NULL : 널 값을 가지지 않게 합니다.

    - UNIQUE : 동일한 애트리뷰트 값을 갖는 튜플이 두 개 이상 존재하지 않도록 보장합니다.
    즉, `중복 불가능`을 뜻합니다.  

    - DEFAULT : DEFAULT 뒤에 특정 값을 지정하여 NULL값 대신에 초기화 시켜줄 수 있습니다.

    - CHECK : 한 애트리뷰트가 가질 수 있는 값들의 범위를 지정합니다.

  ```sql
  create table employee (
    id int(5) unsigned not null auto_increment,
    name varchar(4),
    salary int(10),
    manage_salary int(10),
    check (manage_salary > salary)
  ) auto_increment = 1001;
  ```

  - __기본키 제약조건__

    기본키(Primary key)는 엔티티 무결성 제약조건에 의해 널 값을 갖지 않아야 합니다.
    기본키는 각 릴레이션마다 최대 1개의 기본키를 지정할 수 있습니다.

  - __참조 무결성 제약 조건__

    한 릴레이션에 들어 있는 외래키의 개수만큼 참조 무결성 제약조건을 명시 할 수 있습니다.
    참조 무결성 제약조건을 보존하기 위해 DB가 갱신된 후에 검사를 수행해야 합니다.

## DDL

  데이터 정의어(Data Definition Language)의 역할은 다음과 같습니다.

  1. 스키마의 생성과 제거
  2. 릴레이션 정의
  3. 테이블에 속성(애트리뷰트) 추가, 변경
  4. 인덱스 생성
  5. 도메인 생성

  데이터 정의어의 종류는 다음과 같습니다.

  1. CREATE : DOMAIN, TABLE, VIEW, INDEX
  2. ALTER : TABLE
  3. DROP : DOMAIN, TABLE, VIEW, INDEX
  4. TRUNCATE : DROP후 CREATE

  > 두문자 : C - A - D

  -----------------------------------------------------------------------------

### CREATE

  - __스키마의 생성과 제거__

  스키마는 릴레이션, 도메인, 제약조건, 뷰 등을 그룹화 한 것입니다.

  ```sql
  create schema my_schema authorization John;
  ```

  John 계정의 사용자가 my_schema 생성

  스키마 생성 후 스키마 내에 릴레이션을 생성 할 수 있습니다. 스키마 제거는 아래와 같습니다.
  단, 스키마가 비어 있어야 삭제가 가능합니다.

  ```sql
  drop schema my_schema;
  ```

  - __릴레이션 정의__

  릴레이션(테이블)을 만드는 것은 아래와 같습니다.

  ```sql
  CREATE TABLE wp_options (
  	option_id bigint(20) unsigned NOT NULL AUTO_INCREMENT,
 	  option_name varchar(64) NOT NULL DEFAULT '',
  	option_value longtext NOT NULL,
  	autoload varchar(20) NOT NULL DEFAULT 'yes',
  	PRIMARY KEY (option_id),
  	UNIQUE KEY option_name (option_name)
  ) ENGINE=MyISAM AUTO_INCREMENT=1203 DEFAULT CHARSET=utf8
  ```

  위 처럼 릴레이션을 생성하면서 `제약조건`을 줄 수도 있습니다.

  > unsigned : 음수는 사용 안하고 양수 값만 사용하겠다는 의미입니다.
  >
  > NOT NULL : Null값은 허용하지 않겠다는 의미입니다.
  >
  > AUTO_INCREMENT : 값을 자동으로 생성해주겠다는 의미입니다.
  >
  > DEFAULT : 값을 넣지 않았을때 기본으로 설정되는 값입니다.

  `AUTO_INCREMENT`를 위에서 지정하고 닫는괄호`)`뒤에 AUTO_INCREMENT 초기값을 정해야 합니다.

  - __2개의 컬럼으로 기본키 지정__

  ```sql
  CREATE TABLE invoiceproduct (
  	pInvoiceId varchar(20) NOT NULL DEFAULT "",
  	ipProductId int(5) unsigned NOT NULL,
  	ipQuantity int(255) unsigned NOT NULL,
  	PRIMARY KEY (pInvoiceId, ipProductId),
  	FOREIGN KEY (pInvoiceId) REFERENCES invoice(vId),
	  FOREIGN KEY (ipProductId) REFERENCES storage(pId)
  ) AUTO_INCREMENT=5000001 DEFAULT CHARSET=utf8;
  ```

  - __외래키 참조__

  사용방법 : FOREIGN KEY (컬럼명) REFERENCES 참조테이블명(컬럼명)

  ```sql
  CREATE TABLE bbs (
  	id int(6) unsigned NOT NULL AUTO_INCREMENT,
  	memberid int(6) unsigned NOT NULL,
 	  title varchar(50) NOT NULL,
  	date datetime NOT NULL DEFAULT current_timestamp,
  	content varchar(100),
  	PRIMARY KEY (id),
  	FOREIGN KEY (memberid) REFERENCES member(id)
  ) AUTO_INCREMENT=100001 DEFAULT CHARSET=utf8;
  ```

  - __외래키 여러개 지정__

  ```sql
  CREATE TABLE soldProduct (
  	serial int(5) unsigned NOT NULL AUTO_INCREMENT,
  	soldInvId varchar(20) NOT NULL DEFAULT "",
  	soldShopId int(5) unsigned NOT NULL,
  	soldTransportId int(5) unsigned NOT NULL,
    soldId int(5) unsigned NOT NULL,
  	soldQuantity int(255) unsigned NOT NULL,
  	soldTotalPrice int(10) unsigned NOT NULL,
  	soldDate datetime NOT NULL DEFAULT current_timestamp,
  	chargeState varchar(10) DEFAULT "미청구",
  	transportState varchar(10) DEFAULT "미지급",
    PRIMARY KEY (serial),
  	FOREIGN KEY (soldId) REFERENCES storage(pId),
  	FOREIGN KEY (soldInvId) REFERENCES invoice(vId),
  	FOREIGN KEY (soldTransportId) REFERENCES admins(aId),
  	FOREIGN KEY (soldShopId) REFERENCES admins(aId)
  )AUTO_INCREMENT=80000001 DEFAULT CHARSET=utf8;
  ```

  - __인덱스 생성__

  인덱스는 검색 성능을 향상시키기 위해 사용되며, 뷰(View)는 인덱스를 생성할 수 없습니다.
  하지만, 인덱스는 저장공간을 잡아먹기 때문에 연산 속도를 저하 시킵니다.

  인덱스의 디폴트는 오름차순 입니다.

  ```sql
  create index deptnoIndex on department(deptno);
  ```

  - __도메인 생성__

  ```sql
  create domain deptname char(10) default '개발';
  ```

  - __View 생성__

  > CREATE VIEW 뷰의_이름 AS SELECT 칼럼_이름 FROM 테이블_이름 WHERE 조건;

  아래는 View 생성 예제 입니다.

  ```sql
  create view millioncity as select name from city where population > 1000000;
  ```

  -----------------------------------------------------------------------------

### 데이터 타입

  **데이터 타입**  

  |종류    |특징|
  |--------|----|
  |INT|  정수형|
  |SMALLINT|  작은 정수형|
  |BIGINT| 큰 정수형|
  |REAL|  실수형|
  |FLOAT(N)|  적어도 N개의 숫자가 표현되는 실수 형|
  |CHAR(N)|  N바이트의 문자열, 생략시 1|
  |VARCHAR(N)|  최대 N개까지의 문자열을 표현가능한, 가변 문자열|
  |DATETIME|  날짜형|
  |BLOB| Binary Large OBject, 멀티미디어 데이터 등을 저장|

  -----------------------------------------------------------------------------

### TRUNCATE / DROP

  테이블의 구조를 유지하며 모든 데이터(행)을 삭제하기 위해서는 `TRUNCATE`를 사용하면됩니다.
  TRUNCATE을 사용하면 복원이 불가능 합니다.
  테이블 자체를 삭제하기 위해서는 `DROP`을 사용하면 됩니다.

  > TRUNCATE TABLE 테이블명
  >
  > DROP TABLE 테이블명

  DROP을 사용할때 참조 관계 테이블 삭제 시 `CASCADE CONSTRAINT` 옵션을 사용해서
  테이블 구조를 모두 삭제할 수 있습니다.

  -----------------------------------------------------------------------------

### ALTER

  ALTER는 TABLE에만 적용이 가능합니다. ALTER는 기존 릴레이션에 애트리뷰트(속성)를
  추가하거나 삭제할때 사용되는 SQL문입니다.

  **ALTER TABLE**  

  |종류    |특징|
  |--------|----|
  |ADD|  애트리뷰트 추가|
  |DROP|  애트리뷰트 삭제|
  |CHANGE| 컬럼명, 컬럼 자료형 변경|
  |MODIFY|  컬럼 순서 변경|

  - __애트리뷰트 추가(ADD)__

  alter table [테이블명] add [컬럼명] 자료형 # 맨 뒤에 추가

  alter table [테이블명] add [컬럼명] 자료형 first; # 맨 앞에 추가

	alter table [테이블명] add [컬럼명] 자료형 after [앞 컬럼명]; # 해당 컬럼 뒤에 추가

  > alter table department add dno int(5) not null;

  - __애트리뷰트 삭제(DROP)__

  alter table [테이블명] drop [컬럼명];

  - __애트리뷰트명, 애트리뷰트 자료형 변경(CHANGE)__

  alter table [테이블명] change [기존 컬럼명] [새 컬럼명] 자료형; # 컬럼명과 자료형 변경

	alter table [테이블명] change [컬럼명] [컬럼명] 새 자료형;  # 자료형만 변경

  > alter table department change dno deptno int(8);
  >
  > alter table department change deptno deptno int(5) unsigned auto_increment;

  - __애트리뷰트 순서 바꾸기, 타입, 길이 변경(MODIFY)__

  alter table [테이블명] modify [컬럼명] 자료형 first;

	alter table [테이블명] modify [컬럼명] 자료형 after [다른 컬럼명];

  > alter table department modify deptno int(5) not null after name;

  alter table [테이블명] modify(컬럼명 datatype);

  > alter table emp modify(address varchar(30));
  >
  > alter table emp modify(comm number(9) CONSTRAINT comm_nn NOT NULL);

  - 제약조건 추가 및 삭제

  > alter table [테이블 명] add constraint 제약조건이름 제약조건;

  ```sql
  alter table student add constraint student_pk primary key (stdno);
  ```

  > alter table [테이블 명] drop constraint 제약조건이름;

  -----------------------------------------------------------------------------

## DML

  데이터 조작어(Data Manipulation Language)의 역할은 데이터를 조작(조회, 추가, 변경, 삭제) 하기 위해 사용하며,
  사용자가 응용프로그램과 데이터베이스 사이에 실질적인 데이터 처리를 위해 사용됩니다.

  DML의 종류는 아래와 같습니다.

  1. SELECT
  2. INSERT
  3. DELETE
  4. UPDATE

### SELECT

  SELECT문은 테이블을 구성하는 튜플들 중에서 전체 또는 조건을 만족하는 튜플을 `검색`하여
  `주기억장치 상에 임시테이블을 구성`시키는 명령문입니다.

  - Architecture

  ```sql
  SELECT predicate [테이블명1].컬럼명 [테이블명2].컬럼명
  FROM 테이블명1, 테이블명2
  WHERE 검색 조건
  GROUP BY 컬럼명1, 컬러명2 ...
  HAVING 조건
  ORDER BY 속성명 [ASC/DESC];
  ```

  __!! 처리 순서 !!__

  > FROM - WHERE - GROUP BY - HAVING - SELECT - ORDER BY (FWGHSO)

  - SELECT 절

    `predicate`는 불러올 튜플 수를 제한 할 명령어를 기술 합니다.

    > predicate 종류
    >
    > all : 모든 튜플 검색, 주로 생략
    >
    > distinct : 중복된 튜플이 있으면 그 중 한개만 검색. 즉, 중복 제외
    >
    > distinctrow : 중복된 튜플을 검색하지만 선택된 속성의 값이 아닌, 튜플 전체를 대상.

    속성명에 * 를 기술하면 전체 컬럼명을 나타냅니다.

  - FROM 절

    질의에 의해 검색될 데이터들을 포함하는 테이블 명을 기술합니다.

  - WHERE 절

    검색할 조건을 기술합니다.

  - GROUP BY 절

    특정 속성을 기준으로 그룹화하여 검색할 때 그룹화 할 속성을 지정합니다.
    일반적으로 그룹함수와 같이 사용됩니다.

    **그룹함수**  

    |종류    |특징|
    |--------|----|
    |SUM|  그룹별 합계|
    |COUNT|  그룹별 튜플 수|
    |MAX| 그룹별 최댓값|
    |MIN|  그룹별 최솟값|
    |AVG|  그룹별 평균|

  - HAVING 절

    GROUP BY와 함께 사용되며, `그룹에 대한 조건을 지정`합니다.

  - ORDER BY 절

    특정 속성을 기준으로 정렬하여 검색할 때 사용합니다.

    ASC는 오름차순이며 생략가능하고, DESC는 내림차순을 나타냅니다.

  EX) 등록 테이블("FEE")에서 장학금을 1000000이상 지급받은 학생 중에서 2회 이상 지급받은
  학생의 학번과 지급받은 횟수를 학번 내림차순으로 출력하시오.

  ```sql
  select stu_no, count(*)
  from fee
  where jang_total >= 1000000
  group by stu_no
  having count(*) > 1
  order by stu_no desc;
  ```

  조건이 2개 이상이면 having절을 사용해야 하는지 생각 해봐야 합니다.

  EX) 각 학생의 학번과 이름, 수강년도, 학기, 수강과목코드, 교수코드를 나타내시오.

  ```sql
  select student.stu_no, stu_name, att_year, att_term, sub_code, pref_code
  from student.attend
  where student.stu_no = attend.stu_no;
  ```

  -----------------------------------------------------------------------------

### 문자열 비교

  문자열을 비교하는방법은 `LIKE` 비교 연산자를 사용합니다.

  `%`는 0개 이상의 문자열과 대치, `_`는 임의의 한 개 문자를 나타냅니다.

  > LIKE 'D%' - 첫 문자가 D이고 다음 0개 이상의 문자열이 올 수 있습니다.
  >
  > LIKE 'D___' - 첫 문자가 D이고 다음 3개의 문자열이 옵니다.

### 비교 연산자

  비교연산자는 (=, <, >, >=, <=, <>)가 있습니다.

  `<>`는 자바의 NOT과 같습니다.

  또한 WHERE절에서 `BETWEEN A AND B`를 이용하여 비교할 수도 있습니다.

  > WHERE salary between 1000000 and 2000000;

### IN & NULL

  IN (리스트1, 리스트2 .....)는 컬럼이 리스트내의 값에 속하는지 확인합니다.

  > WHERE deptno IN (1, 2, 3, 4, 5);

  NULL값을 비교하기 위해서는 `IS NULL`이라는 키워드를 사용 해야 합니다.

  > WHERE deptno IS NULL;

### alias

  alias는 별칭으로서 다음과 같은 방식으로 사용할 수 있습니다.

  만약 서로 다른 릴레이션에 같은 이름의 애트리뷰트가 있을 경우 구분하는 방법은 아래와 같습니다.

  1. 애트리뷰트 이름 앞에 릴레이션의 이름을 붙인다.
  2. 튜플변수(Tuple Variable)를 사용합니다. (별칭이라고도 부릅니다.)

  별칭(alias)는 `AS`키워드를 사용합니다.

  > FROM employee as e, department as d
  >
  > e.no = d.no

  -----------------------------------------------------------------------------

### INSERT & UPDATE

  INSERT는 애트리뷰트 값을 추가할 때 사용합니다.

  > INSERT INTO (컬럼명1, 컬럼명2 .....) VALUES ('값1', '값2' .....);

  ```sql
  insert into city (name, countrycode, district, population)
	values ('Hwasong', 'KOR', 'Kyonggi', 312345);
  ```

  UPDATE는 기존에 저장되어있는 애트리뷰트 값을 변경할 때 사용합니다.

  > UPDATE 테이블명 SET 컬럼명='바꿀 애트리뷰트값' ..... WHERE 조건

  ```sql
  update city set name='Siheung', population=153443
	where countrycode='kor' and name='Shihung';
  ```

## DCL

  데이터 제어어(Data Control Language)는 데이터의 보안, 무결성, 회복, 병행 수행제어 등을 정의할때 사용합니다.
  데이터 제어어의 종류는 다음과 같습니다.

  1. COMMIT : 트랜잭션의 작업결과를 반영
  2. ROLLBACK : 트랜잭션 작업 취소 및 복구, 저장 후에는 복구가 되지 않습니다.
  3. GRANT : 사용자 권한 부여
  4. REVOKE : 사용자 권한 회수

### 사용자 권한

  - 사용자 생성 : create user 사용자명 identified by '비밀번호';

  > host명
  >
  > %는 localhost가 아닌 원격에서 접속 가능
  >
  > localhost는 localhost, 로컬 컴퓨터에서 접속 가능

  - 사용자 권한 부여 : grant

    1. grant all privileges on 데이터베이스명.* to 사용자명;
    2. grant 부여할 권한 SQL명령문 on 데이터베이스명.* to 사용자명;

  - 사용자 권한 회수 : revoke

    revoke sql명령문 on 데이터베이스명.* from '해당유저이름';

  - 사용자 삭제

    1. drop user '해당 유저 이름'
    2. delete from user where user='해당유저이름';
    3. delete from db where user='해당유저이름';

    ```sql
    drop user kim@localhost;
    delete from user where user='lee';
    ```

## Join(조인연산)

  조인연산에는 `Inner Join, Outter Join, Cross Join, Self Join`이 있습니다.

  INNER 조인은 교집합 연산이라고 생각하시면 됩니다.  INNER와 OUTTER는 생략 가능 합니다.

  조인연산에서 `WHERE R.A <비교연산자> S.B`에서 R.A와 S.B는 기본키와 외래키 관계를 가집니다.

  ![join](/images/posts/201904/join.jpg)

  - __Architecture__

  ```
  SELECT
  테이블별칭.조회할칼럼,
  테이블별칭.조회할칼럼
  FROM 기준테이블 별칭
  조인연산자 조인테이블 별칭 ON 기준테이블별칭.기준키 = 조인테이블별칭.기준키 .....
  ```

  - __INNER JOIN__

  ```sql
  SELECT gg._id, gg.name, s.title
  FROM girl_group AS gg
  INNER JOIN song AS s 		
  ON s._id = gg.hit_song_id;

  SELECT c.continent, c.name, t.Name, t.Population
  FROM country AS c
  INNER JOIN city AS t 		
  ON c.code = t.countrycode
  where c.continent = 'Europe'
  order by t.Population desc limit 10;
  ```

  - __INNER JOIN 여러개 사용 및 date_format 함수 사용__

  ```sql
  select o.oId, o.oAdminId, o.oProductId, o.oQuantity, o.oTotalPrice, o.oDate, o.oState, a.aName, p.pName
  from p_order as o
	inner join admins as a
	on o.oAdminId=a.aId
	inner join storage as p
	on o.oProductId=p.pId
	where date_format(oDate, '%Y-%m')=?
	order by o.oDate desc;
  ```

  - __OUTTER JOIN__

  OUTTER JOIN은 크게 LEFT와 RIGHT 2가지가 있습니다.

  출력 : 기준테이블의 값 + 테이블과 기준테이블의 중복된 값

  ```sql
  SELECT gg._id, gg.name, s.title
	FROM girl_group AS gg
	LEFT OUTER JOIN song AS s 	
	ON s._id = gg.hit_song_id;

  SELECT s._id, s.title, gg.name
	FROM girl_group AS gg
	RIGHT OUTER JOIN song AS s
	ON s._id = gg.hit_song_id;
  ```

  A LEFT JOIN B : 교집합을 포함한 A만 나타낸다. (= LEFT OUTER JOIN)

  A RIGHT JOIN B : 교집합을 포함한 B만 나타낸다. (= RIGHT OUTER JOIN)

  ```sql
  select co.continent as ‘대륙명’, co.Name as ‘국가명’,
    city.Name as ‘도시명’, city.Population as ‘인구수’
    from country as co inner join city
    on co.Code=city.CountryCode
    where continent='africa'
    order by city.Population desc limit 10;
  ```

  - __CROSS JOIN__

  CROSS JOIN은 모든 경우의 수를 표현 해줍니다. 방식은 아래 2가지 중 하나를 사용하면 됩니다.

  ```sql
  SELECT
  A.no,
  B.name
  FROM TABLE A
  CROSS JOIN JOIN_TABLE B

  SELECT
  A.no,
  B.name
  FROM TABLE A, JOIN_TABLE B
  ```

  - __SELF JOIN__

  SELF JOIN은 자기자신과 자기자신을 조인하는 것입니다.

  실제로는 한 릴레이션에 접근되지만, FROM 절에 두 릴레이션이 참조되는 것처럼 나타내기
  위해서 별칭을 두 개 지정해야 합니다.

  EX) 모든 사원에 대해서 사원의 이름과 직속 상사의 이름을 검색하시오.

  ```sql
  SELECT E.EMPNAME, M.EMPNAME
  FROM EMPLOYEE E, EMPLOYEE M
  WHERE E.MANAGER = M.EMPNO;
  ```

## 함수

 다음은 순위, 집계, 행 순서, 그룹 내 비율을 구하는 함수를 배워 봅시다.

### 순위 함수

  순위함수에는 RANK, DENSE_RANK, ROW_NUMBER 등이 있습니다.

  - __RANK__

    RANK 함수는 order by를 포함한 query문에서 특정 컬럼에 대한 순위를 구하는 함수 입니다.
    특정 `범위(Partition)`내에서 순위를 구할 수 있고, 전체 데이터에 대한 순위도 구할 수 있습니다.
    또한 동일한값에 대해서는 동일한 순위를 부여합니다.

    > SELECT(조회대상, RANK()) OVER(PARTITION BY(그룹기준1) ORDER BY(그룹기준2))(RANK 컬럼) FROM(테이블);

  - __DENSE_RANK__

    비교 대상과 동일값을 하나로 설정하여 순위를 부여합니다.

    > SELECT(조회대상, RANK()) OVER(ORDER BY(그룹기준1)) RANK, DENSE_RANK() OVER(ORDER BY(그룹기준2))(DENSE_RANK 컬럼) FROM(테이블);

  - __ROW_NUMBER__

    비교 대상과 동일값에 임의의 유니크 값을 부여하여 순위를 구합니다.

    > SELECT(조회대상, RANK()) OVER(ORDER BY(그룹기준1)) RANK, ROW_NUMBER() OVER(ORDER BY(그룹기준2))(ROW_NUMBER 컬럼) FROM(테이블);

  -----------------------------------------------------------------------------

### 집계함수

  sum(컬럼명), max(컬럼명), min(컬럼명), avg(컬럼명), count(컬럼명) 이름을 보면 알 수 있듯이
  합계, 최댓값, 최솟값, 평균, 개수를 구하는 함수들 입니다.

  ```sql
  select continent, count(name), sum(population), avg(gnp)
  from country
  group by continent
  order by avg(gnp) desc;
  ```

  -----------------------------------------------------------------------------

### 행 순서 함수

  행 순서 함수에는 FIRST_VALUE, LAST_VALUE, LAG, LEAD 함수가 있습니다.

  - __FIRST_VALUE__

    정렬한 대상에서 최초로 나온 값을 추출합니다.

    > SELECT(조회대상, FIRST_VALUE(추출대상)) OVER(PARTITION BY(대상 파티션) ORDER BY(정렬기준) ASC ROWS(추출범위)) AS(추출컬럼) FROM(테이블);

  - __LAST_VALUE__

    정렬한 대상에서 가장 후에 나온 값을 추출합니다.

    > SELECT(조회대상, LAST_VALUE(추출대상)) OVER(PARTITION BY(대상 파티션) ORDER BY(정렬기준) ROWS(추출범위)) AS(추출컬럼) FROM(테이블);

  - __LAG__

    지정된 순서(선행)의 데이터(행)를 추출합니다.

    - 정렬된 대상에 한행 앞선 데이터(행) 추출

    > SELECT(조회대상, LAG(추출대상)) OVER(ORDER BY(정렬기준)) AS(추출 컬럼) FROM(테이블) WHERE(조건);

    - 정렬된 대상에 지정 행 앞선 데이터(행)를 추출하되 추출값이 NULL인 경우 처리

    > SELECT(조회대상, LAG((추출대상),(선행범위),(NULL값 처리)) OVER(ORDER BY(정렬기준)) AS(추출컬럼) FROM(테이블) WHERE(조건);

  - __LEAD__

    지정된 순서(후행)의 데이터(행)를 추출합니다.

    > SELECT(조회대상, LEAD((추출대상),(후행범위)) OVER(ORDER BY 정렬기준) AS "추출컬럼" FROM(테이블);

  -----------------------------------------------------------------------------

### 그룹 내 비율 함수

  - __RATIO_TO_REPORT__

    전체에서 대상이 차지하는 비율을 출력할 수 있습니다.

    > SELECT(조회대상, ROUND(RATIO_TO_REPORT(전체대상) OVER(), 2) AS(추출컬럼) FROM(테이블) WHERE(조건);

  - __PERCEONT_RANK__

    순위를 비율로 출력할 수 있습니다.

    > SELECT(조회대상, PERCENT_RANK() OVER(PARTITION BY(대상 파티션) OERDER BY(정렬기준)) AS(추출컬럼) FROM(테이블);

  - __CUME_DIST__

    대상과 같거나 이하의 값에 대한 누적 백분율을 출력할 수 있습니다.

    > SELECT(조회대상, CUME_DIST() OVER(PARTITION BY(대상 파티션) ORDER BY(정렬 기준)) AS(추출컬럼) FROM(테이블);

  - __NTILE__

    정렬된 대상을 지정된 수로 분할할 수 있습니다.

    > SELECT(조회대상, NTLE((분할수)) OVER(ORDER BY(정렬기준)) AS(추출컬럼) FROM(테이블);
