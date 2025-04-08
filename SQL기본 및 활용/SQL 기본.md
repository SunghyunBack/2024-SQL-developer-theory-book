
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
     


 ★ SELECT문 실행 순서
   - FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY 순으로 실행된다.



- 명시적(Explicit) 형변환과 암시적(Import) 형변환
  - 두 개의 데이터의 데이터 타입(형)이 일치하도록 변환하는것이다.
  - 예를 들어 숫자와 문자열, 문자열과 날짜형 비교와 같이 데이터 타입이 불일치할때 일어난다.
  - 명시적, 암시적 형변환 2개로 나뉘어 진다.
  - 명시적 형변환 : 형변환 함수를 사용해서 데이터 타입을 일치시킨다.
    - TO_NUMBER(문자열) : 문자열을 숫자열로 변환
    - TO_CHAR(숫자 혹은 날짜, [FORMAT]) : 숫자 혹은 날짜를 지정된 FORMAT의 문자로 변환한다.
    - TO_DATE(문자열,FORMAT) : 문자열을 지정된 FORMAT의 날짜형으로 변환한다.
  - 암시적 형변환 : 개발자가 형변환을 하지 않은 경우 데이터베이스 관리 시스템이 자도응로 형변환하는것을 의미한다.


  TIP - 인덱스 칼럼에 형변환을 수행하면 인덱스를 사용하지 못한다.!!
   - 데이터를 빠르게 조회하기 위해 인덱스 키를 기준으로 정렬해 놓은 데이터이다.
   - 기본적으로 인덱스는 변형이라는것이 발생한다면 사용할수 없다. 물론 예외도 있음
   ```
   ex)
   SELECT *
   FROM EMP
   WHERE EMPNO='100';

   다음과 같을 대  EMPNO 칼럼은 숫자형 타입이고 기본키이므로 자동으로 인덱스가 있다.
   **하지만** 암시적 형변환으로 EMPNO가 TO_CHAR(EMPNO)로 변환되어 인덱스를 사용할수 없다.
   이와 같은 문제는 명시적 형변환을 사용하여 WHERE EMPNO =TO_NUMBER('100')으로 사용하면 인덱스도 문제 없이 사용할수 있다.
   ```




- 내장형 함수(BUILT-IN Function)

  - 내장형 함수
    - 모든 데이터베이스는 SQL에서 사용할수 잇는 내장형 함수를 가지고 있다.
    - 데이터베이스 관리 시스템 벤더별로 약간의 차이가 있지만, 거의 비슷한 방법으로 사용이 가능하다.
    - 형변환 함수, 문자열 및 숫자형 함수, 날짜형 함수가 있다.
   
  - DUAL 테이블
    - DUAL 테이블은 ORACLE 데이터베이스에 의해서 자동으로 생성되는 테이블이다.
    - 사용자가 임시로 사용할수 잇는 테이블로 내장형 함수를 실행할 때도 사용할수 있따.
    - ORACLE은 기본적으로 DUAL테이블이라는 Dummy 테이블이 존재한다.

  - 내장형 함수의 종류
    - DUAL 테이블에서 문자형 내장형 함수를 사용하면 다음과 같다
    - ASCII함수는 무자에 대한 ASCII 코드 값을 알려준다. A를 기주능로(65)이다.
    - SUBSTR함수는 지정된 위치의 문자열을 자르는 함수이다.
    - LENGTH함수, LEN함수는 문자열의 길이를 계산한다.
    - LTRIM함수를 사용하면 문자열의 왼쪽 공백을 제거할수 있다.
    - 함수를 중첩해서 사용가능한다. ex) LENGTH(LTRIM('ABC'))
   
    ```
    ex)
    SELECT ASCII('a'), SUBSTR('ABC',1,2),
           LENGTH('A BC'), LTRIM(' ABC'),
           LEGNTH(LTRIM(' ABC'))
    FROM DUAL;

    결과값은 다음과 같다.
    ```
   |ASCII|SUBSTR('ABC',1,2)|LENGTH('ABC')|LTRIM('ABC')|LEGNTH(LTRIM('ABC')|
  |---|---|---|---|---|
  |97|AB|4|ABC|3|




 - 문자열 함수
   - ASCII(문자) : 문자 혹은 숫자를 ASCII 코드값으로 변환한다.
   - CHR/CHAR(ASCII 코드값) : ASCII 코드값으 문자로 변환한다. 오라클은 CHR사용, 다른곳은 CHAR사용
   - SUBSTR(문자열,m,n) : 문자열 m번째 위치부터 n개를 짜른다.
   - CONCAT(문자열1, 문자열2) : 문자열 1번과 문자열 2번을 결합한다. ORACLE은 '||' 다른곳은 '+'사용
   - LOWER(문자열) : 영문자를 소문자로 변환한다.
   - UPPER(문자열) : 영문자를 대문자로 변환한다.
   - LENGTH 혹은 LEN(문자열) : 공백을 포함한 길이를 알려준다.
   - RTRIM(문자열, 지정문자) : 오른쪽에서 지정된 문자를 삭제한다. 지정된 문자를 생략하면 공백을 삭제한다.
   - TRIM(문자열, 지정된 문자) : 왼쪽 미 오른쪽 에서 지정된 문자를 삭제한다. 지정된 문자를 생략하면 공백을 삭제한다.

 - 날짜형 함수
   - 오늘 날짜를 구하기 위해서는 SYSDATE를 사용하면 된다.
   - 해당 연도만 알고 싶다면, EXTRACT함수를 사용한다.
   - TO_CHAR함수는 형변환 함수 중에서 가장 많이 사용하는것으로 숫자나 날짜를 원하는 포맷의 문자열로 변환한다.
  
     ```
     ex)
     SELECT SYSDATE,
      EXTRACT(YEAR From sysdate),
      TO_CHAR(SYSDATE, 'YYYMMDD')
      FROM DUAL;

     결과값은 다음과 같다.
     ```
   |SYSDATE|EXTRACT(YEAR From sysdate)|TO_CHAR(SYSDATE, 'YYYMMDD')|
   |---|---|---|
   |25/04/08|2025|20250408|

     - SYSDATE : 오늘의 날짜를 날짜 타입으로 알려준다.
     - EXTRACT(YEAR FROM SYSDATE) : 날짜에서 년, 월, 일을 조회한다.

 - 숫장형 함수
   - 절대값을 계산하는 ABS함수와 음수, 0, 양수를 구분하는 SIGN, 나머지를 계산하는 MOD등의 함수를 사용할수 있다.
       ```
     ex)
     SELECT ABS(01), SIGN(10),
      MOD(4,2),CEIL(10.9), FLOOR(10.1),
      ROUND(10.222,1)
      FROM DUAL;

     결과값은 다음과 같다. 
     ```
   |ABS(01)|SIGN(10)|MOD(4,2)|CEIL(10.9)|FLOOR(10.1)|ROUND(10.222,1)|
   |---|---|---|---|---|---|
   |1|1|0|11|10|10.2|
   - ABS(숫자) : 절대값
   - SING(숫자) : 양수, 음수, 0을 구별한다.
   - MOD(숫자1,숫자2) : 숫자 1을 숫자2로 나누어 나머지 계산. %사용가능하다
   - CEIL/CEILNG(숫자) : 숫자보다 크거나 같은 최소의 정수
   - FLOOR(숫자) : 숫자보다 작거나 같은 최대의 정수
   - ROUND(숫자,m) : 소수점 m자리에서 반올림
   - TRUNC(숫자,m) : 소수점 m자리에서 절삭한다.


- DECODE와 CASE 문
  - DECODE
    -IF문을 구현할수 있다. 즉 특정 조건이 참이면 A를 거짓이면 B로 응답한다.

    ```
    DECODE(EMPNO,1000, 'TRUE','FALSE)

    EMPNO의컬럼의 각 행 값과 1000을 비교하여 동일하면 TRUE를 틀리면 FALSE를 출력한다.
    ```

  - CASE문
    - IF ~ THEN ~ ELSE - END의 프로그래밍 언어처럼 조건문을 사용할수 있다.
    - 조건을 WHEN구에 사용하며, 해당 조건이 참이면 THEN이 실행되고 거짓이면 ELSE구가 실행된다.
   
      ```
      SELECT CASE
             WHEN EMPNO =1000 THEN 'A'
             WHEN EMPNO =1001 THEN 'B'
             ELSE 'C'
            END
      FROM EMP
      ```

- WHIT 구문
  - 서브쿼리(Subquery)를 사용해서 임시 테이블이나 뷰처럼 사용할수 잇는 구문이다.
  - 서브쿼리 블록에 별칭을 지을수 있따.
  - 옵티마이저는 SQL을 인라인 뷰나 임시 테이블로 판단한다.
 
    ```
    (SELECT * FROM EMP
     UNION ALL
    SELECT * FROM EMP
    )
    SELECT * FROM viewData WHERE EMPNO= 1000;

    결과값은 다음과 같다. 
    ```
   |EMPNO|ENAME|DEPTNO|MGR|JOB|SAL|
   |---|---|---|---|---|---|
   |1000|임베스트|20||CLERK|800|
   
