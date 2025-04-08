
# 관계형 데이터베이스(Relatioin DataBase)
 - 1970년 E.F. Codd박사의 논문에 처음 소개 되었다
 - 릴레이션(Relation)과 조인 연산을 통해 합집합, 교집합, 차집합 등을 만들수 있다.
TIP - 릴레이션(Relation)은 데이터들의 표(Table)의 형태로 표현한것으로 구조를 나타내는 릴레이션 스키ㅏ와 실제 값들인 릴레이션 인스턴스로 구성된다.
![image](https://github.com/user-attachments/assets/655281af-91ae-4d66-8b65-1d9517bfba82)


● 데이터베이스와 데이터베이스 관리 시스템의 차이
- 데이터베이스는 데이터를 어떠한 형태의 자료구조(Data Structrue)로 사용하느냐에 따라서 나누어진다.
- 데이터베이스의 종류로는 계층형, 네트워크형, 관계형 등이 있다.
![image](https://github.com/user-attachments/assets/726d4e40-81e0-4c28-9c49-80ca624dd439)

- 데이터베이스 관리 시스템(Database Management System)은 데이터베이스 자료구조등을 관리하기 위한 소프트웨어를 의미하면 DBMS라고 한다.
- DBMS의 종류는 오라클 등 잇으며 모두 관계형 데이터베이스를 지원한다. 

● 관계형 데이터베이스 집합 연산과 관계 연산
- 집합연산
  - 합집합(Union) : 2개의 릴레이션을 하나로 합하는것, 중복된 행(튜플)은 한번만 조회
  - 차집합(Difference) : 본래 릴레이션에는 존재하고 다른 릴레이션에는 존재하지 않는것을 조회
  - 교집합(Intersection) : 두 개의 릴레이션 간에 공토된 것을 조회한다.
  - 곱집합(Cartesian product) : 각 릴레이션에 존재하는 모든 데이터를 조합하여 연산한다.
 
- 관계연산
  - 선택 연산(Selection) : 릴레이션에서 조건에 맞는 행(튜플)만을 조회한다.
  - 투명 연산(Projection) : 릴레이션에서 조건에 맞는 속성만을 조회한다.
  - 결합 연산(Join) : 여러 릴레이션의 공통된 속성을 사용해서 새로운 릴레이션을 만들어 낸다.
  - 나누기 연산(Division) : 기준 릴레이션에서 나누는 릴레이션이 가지고 있는 속성과 동일한 값을 가지는 행(튜플)을 추출하고 나누는 릴레이션의 속성을 삭제한 후 중복된 행을 제거하는 연산
 

- 테이블
  ![image](https://github.com/user-attachments/assets/c98feabe-ad17-4890-8139-a7f9adf2126c)

 - 기본키(Primary key)는 하나의 테이블에서 유일성과 최소성, Not Null을 만족하면서 해당 테이블을 대표한다. EMP테이블에서는 사원번호가 기본키에 해당된다.
 - 테이블은 행과 칼럼으로 구성되어 있으며 행은 하나의 테이블에 저장되는 값으로 튜플(Tuple)이라고도 한다.
 - 칼럼(Column)은 어떤 데이터를 저장하기 위한 필드(Field)로 속성(Attribute)이라고도 한다.
 - 외래키(Foreign key)는 다른 테이블의 기본키를 참조(whdls)하는 칼럼이다. EMP 테이블에서는 부서코드는 DEPT테이블의 기본키인 부서코드를 참조한다.
 - 외래키는 관계 연산 중에서 결합 연산(join)을 하기 위해서 사용한다.



# SQL종류

● SQL(Structured Query Language)
 - 데이터의 구조를 정의, 데이터 조작, 데이터 제어 등을 할수 있는 절차형+비절차형 언어이다.
 - SQL문을 사용하여 데이터베이스를 누구나 쉽게 사용할수 있다.
 - ANSI/ISO 표준을 준수하기 때문에 관리 시스템이 변경되어도 그대로 사용할수 있다.

● SQL 종류

- 종류
 - DDL(Data Definition Language) :  관계형 데이터베이스의 구조를 정의하는 언어 / CREATE, ALTER, DROP, RENAME, TRUNCATE문이 있다.
 - DML(Data Manipulation Language) : 테이블에서 데이터를 입력,수정, 삭제, 조회한다. / INSERT, UPDATE< DELETE, SELECT문이 있다. 
 - DCL(Data COntrol Language) : 데이터베이스 사용자에게 권한을 부여하거나 회수한다. / GRANT, REVOKE문이 있다.
 - TCL(Transaction Control Language) : 트랜잭션을 제어하는 명령어이다. / COMMIT, ROLLBACK, SAVEPOINT문이 있다.

- 트랜잭션(Transaction)
 - 데이터베이스의 작업을 처리하는 단위이다.

 - 트랜잭션의 특성
    - 원자성(Atomicity) : 데이터베이스 연산의 전부가 실행되거나 전혀 실행되지 않아야 한다(ALL OR NOTHING)
    - 일관성(Consistency) : 실행 결과로 데이터베이스의 상태가 모순되지 않아야 한다. 실행 후에도 일관성이 유지되어야 한다.
    - 고립성(Isolation) : 실행 중 생성하는 연산의 ㅈ우간결과는 다른 트랜잭션이 접근할수 없다.
    - 영속성(Durability) : 트랜잭션이 그 실행을 성공적으로 완료하면 그 결과는 영구적 보장이 되어야 한다.
  

- SQL문의 실행 순서
   - 파싱(Parsing) : SQL문의 문법을 확인하고 구문 분석한다.
   - 실행(Execution) : 옵티마이저(Optimizer)가 수립한 실행 계획에 따라 SQL을 실행한다.
   - 인출(Fetch) : 데이터를 읽어서 전송한다.




  ■ SELECT문
  - 테이블에 입련된 데이터를 조회 하기 위해서 사용
  - 특정 칼럼(Column) 혹은 행만 조회할수 잇다.
 
    ```
    SELECT (조회를 원하는 칼럼(Column)을 선택한다.)
    FROM (원하는 테이블을 지정한다 ex: EMP)
    WHERE (원하는 데이터의 조건 지정 ex: 사원번호 = 1000);

    ```
TIP -  칼럼(Column)지정시 '*'로 표현하면 모든 칼럼을 선택한다는 뜻이다.

 - 정렬(ORDER BY=> ASC(오름차순) or DESC(내림차순))
    - ORDER BY를 같이 사용할수 있다.(오름차순(Ascending) 혹은 내림차순(Descending)으로 출력한다.)
    - 정렬의 시점은 모든 실행이 끝난 후에 데이터를 출력해 주기 바로 전이다.
    - ORDER BY는 데이터베이스 메모리를 많이 사용하게 된다. 대량의 데이터에는 성능 저하가 발생한다.
    - ORACLE에서는 SORT_AREA_SIZE를 사용할수 있다.(다만 데이터가 너무 작으면 성능 저하가 발생한다.)
    - 정렬 회피하기 위해서는 인덱스(Index)를 생성할때 사용ㅎ자가 원하는 형태로 오름차순 혹은 내림차순으로 생성해야한다.
    - 기본적으로 오름차순으로 정렬된다.
      
      ```
      ex)
      SELECT *
      FROM EMP
      ORDER BY ENAME (ASC), SAL DESC
      => EMP테이블의 모든 컬럼을 조회하는데 이때 ENAME 컬럼은 오름차순으로 SAL칼럼은 내림차순으로 조회하겠다.
      ```

    - Index를 사용한 정렬 회피
       - 정렬은 ORCLE 데이터베이스에 부하를 주므로, 인덱스를 사용해서 ORDER BY를 회피할수 있다.
     ```
     ex)
     CREATE TABLE EMP(
      empno number(10) primary key,
      ename varchar2(20),
      sal number(10)
     );

     INSERT INTO EMP VALUES(1000, '임베스트', 20000);
     INSERT INTO EMP VALUES(1001, '조조', 20000);
     INSERT INTO EMP VALUES(1002, '관우', 20000);
     ```
       - 다음과 같이 기본키인 empno로인해 자동으로 오름차순 인덱스가 생성된다.

     ```
     another ex) Oracle SQL

     SELECT /*+ INDEX_DESC(A) */
     FROM EMP A;
     ```

     - 다음과 같은 예를 보면 /*+ INDEX_DESC(A) */(내림차순)를 통해 EMP 테이블에서 생성된 인덱스를 내림차순으로 읽게 지정한다.
     - 따라서 ORDER BY 구문을 사용하지 않아도 된다.

    ★ **컬럼**을 정렬할건지 **인덱스**를 정렬한건지는 다른것이다. 주의!!



 - DISTINCT와 Alias
    - DISTINCT는 칼럼(Column)앞에 지정하여 중복된 데이터를 한번만 조회한다.
    - Alias(별치)은 테이블명이나 칼럼(Column)명이 너무 길어서 간략하게 사용할때 사용한다.
  
      ```
      ex)
      SELECT ENAME AS "이름" FROM EMP a
      WHERE a.EMPNO=1000;

      다음과 같을 경우 `a`를 테이블명 처럼 사용하고 칼럼명은 "이름"으로 출력된다.
      ```

 - WHERE문 사용
    - Where문이 사용할수 있는 연산자는 비교, 부정, 논리, SQL, 부정 SQL 연산자가 잇다.
  
    - 비교 연산자
      - = : 같다
      - < : 작다
      - <= : 작거나 같다
      - > : 크다
      - >= : 크거나 같다

    - 부정 비교 연산자
       - != : 같지 않다
       - ^= : 같지 않다
       - <> : 같지 않다
       - NOT 컬럼명 = : 같지 않다 
       - NOT 컬럼명 > : 같지 않다

     - 논리 연산자
       - AND : 조건을 모두 만족해야 참(TRUE)
       - OR : 조건 중 하나만 만족해도 참(TRUE)
       - NOT : 참이면 거짓으로 바꾸고 거짓이면 참으로 바꾼다.
   
     - SQL 연산자
       - LIKE '%비교 문자열%' : 비교 문자열을 조회한다. '%'는 모든 값을 의미한다.
       - BETWEEN A AND B : A와 B 사이의 값을 조회한다.
       - IN(list) : OR를 의미하며 list 값 중에 하나만 일치해도 조회한다.
       - IS NULL : NULL값을 조회한다.
         
     - 부정 SQL 연산자
       - NOT BETWEEN A AND B : A와 B사이의 해당되지 않는 값을 조회한다.
       - NOT IN(list) : list와 불일치한 것을 조회한다.
       - IS NOT NULL : NULL값이 아닌것을 조회한다.
      
    ```
    ex)
    SELECT *
    FROM EMP
    WHERE EMPNO = 1000
     AND SAL >= 1000;
    EMP 테이블에서 컬럼 EMPNO의 값이 1000이며 컬럼 SAL의 값이 1000이상인 행을 조회한다.
    ```


 - LIKE문 사용
   - 와일드 카드를 사용해서 데이터를 조회할수 있다.
  
   - 와일드 카드
     - % : 어떤 문자를 포함한 모든 것을 조회한다. ex) '조%'는 '조'로 시작하는 모든 문자를 조회한다.
     - _(Underscore) : 한 개인 단일 문자를 의미한다.
    
  ```
  ex)
SELECT *
FROM EMP
WHERE ENAME LIKE 'test%";
 EMP테이블에서 칼럼이름이 ENAME의 값중 test가 들어간 모든 행을 조회한다.

"1%" : 1로 끝나는 의미
"%est%" : 중간에 'est'가 있는 모든 것
"test_" : test로 시작하고 하나의 글자만 더 있는 것
만약 LIKE에 와일드 카드를 생략한다면 "="와 같다.
  ```

- BETWEEN문 사용
  - 지정된범위에 있는 값을 조회한다.
  - "BETWEEN 100 and 200"은 100rhk 200을 포함하며 사이의 값을 조회한다.
 
  ```
  SELECT *
  FROM EMP
  WHERE SAL BETWEEN 1000 AND 2000;

  EMP테이블에서 컬럼이름이 SAL인 행중 값이 1000<=  <=2000인 행을 조회한다.

  만약 BETWEEN NOT 1000 AND 2000 => 1000 미만, 2000초과 값을 조횐한다.
  ```

 - IN문 사용
   - "OR"의 의미를 가지고 있어서 하나의 조건만 만족해도 조회가 된다.

   ```
   SELECT *
   FROM EMP
   WHERE JOB IN ('CLERK','MANAGER');

   EMP 테이블에서 JOB 컬럼 중 값이 CLERK 혹은 MANAGER인 값을 조회한다.
   ```


 - NULL값 조회
   - NUll의 특징
    - 모르는 값을 의미
    - 값의 부재를 의미
    - 숫자 혹은 날짜를 더하면 NULL이 된다.
    - 어떤 값을 비교 할때, '알수 없음'이 반환된다.

   - NULL값 조회
     - IS NULL을 사용하여 조회한다.
     - NULL값이 아니 것을 조회하고 싶을때는 IS NOT NULL을 사용한다.
   ```
   ex)
   SELECT *
   FROM EMP
   WHERE MGR IS NULL;
   EMP 테이블에서 MGR 컬럼의 값중 NULL인 행을 조회한다.
   ```
     
  - NULL 관련 함수
    - NVL 함수(ORACLE) : NULL이면 다른 값으로 바꾸는 함수. ex) "NVL(MGR,0)"은 칼럼이 MGR의 값이 NULL이면 0을 반환한다.
    - NVL2 함수(ORACLE) : NVL 함수와 DECODE함수를 하나로 만든 것이다. "NVL2(MGR,1,0)"은 MGR 칼럼이 NULL이 아니면 1을 NULL이면 0을 반환한다.
    - NULLIF 함수 (ORACLE, MS-SQL, MYSQL) : 두개의 값이 같으면 NULL을, 같지 않으면 첫번째 값을 반환한다. ex) "NULLIF(exp1,exp2)"은 exp1과 exp2가 같으면 NULL을 같지 않으면 exp1을 반환한다.
    - COALESCE (ORACLE, MS-SQL) : NULL이 아닌 최초의 인자 값을 반환한다. ex) "COALESCE(exp1,exp2....)"은 exp1이 NULL이 아니면 exp1의 값을, 그렇지 않으면 그 뒤이 값이 NULL여부를판단하여 값을 반환한다.
   


 - GROUP 연산
   - GROUP BY문
     - 테이블에서 소규모 행을 그룹화하여 합계, 평균, 최댓값, 최솟값 등을 게산할수 잇다.
     - HAVING구에 조건문을 사용한다.
     - Grouping된결과에 대한 조건문을 사용한다.
     - ORDER BY를 사용해서 정렬을 할수 있다.
     ```
     ex)
     SELECT DEPTNO,SUM(SAL)
     FROM EMP
     GROUP BY DEPTNO

     EMP테이블에서 DEPTNO의 컬럼을 그룹화 한 후 SAL컬럼에서 그룹화된 것들을 합하여 반환한다.
     ```

   - HAVING문 사용
     - GROUP BY에 조건절을 사용하려면 HAVING을 사용해야한다.
     - 만약 WHERE절에 조건문을 사용하게 되면 조건을 충족하지 못하는 데이터들은 GROUP BY 대상에서 제외된다.

       ```
       ex)
       SELECT EPTNO, SUM(SAL)
       FROM EMP
       GROUP BY DEPTNO
       HAVING SUM(SAL) >10000;

       EMP테이블에서 DEPTNO로 그룹화 한 후 SAL컬럼을 합하여 합친 값이 10000초과인 값 조회
       ```

    - 집계함수
      - COUNT() : 행 수를 조회. COUNT(*) 표현은 NULL을 계산하지만 COUNT(컬럼명)은 NULL값을 제외한다.
      - SUM() : 합계를 계산
      - AVG() : 평균을 계산
      - MAX()와MIN() : 최댓값과 최솟값을 계산
      - STDDEV() : 표준편차를 계산
      - VARANCE() : 분산을 계산

    - GROUP BY 사용 예제
      ```
      ex)
      SELECT DEPTNO, MGR, AVG(SAL)
      FROM EMP
      GROUP BY DEPTNO, MGR;

      EMP 테이블에서 DEPNO와 MGR 컬럼을 그룹홯 한 후 SAL 컬럼의 평균을 계산하여 행을 나타낸다.
      ```

      ```
      ex)
      SLECT JOB, SUM(SAL)
      GROM EMP
      GROUP BY JOB
      HAVING SUM(SAL) >=1000;

      EMP 테이블에서 JOB컬럼으로 그룹화 한 후 컬럼 SAL값 중 1000이상의 행으조회
      ```

      ```
      ex)
      SELECT DEPTNO, SUM(SAL)
      FROM EMP
      WHERE EMPNO BETWEEN 1000 AND 1003
      GROUP BY DEPTNO;

      EMP테이블에서 EMPNO컬럼의 값이 1000이상 1003이하의 값 중 DEPTNO의 값으로 그룹화 한것을 조회한다.
      ```
     
