
1. 서브쿼리(Subquery)
    - Main Query와 Subquery
      - 서브쿼리는 SELECT문 내에 다시 SELECT문을 사용하는 SQL문이다.
      - 서브쿼리의 형태는 FROM구에 SELECT문을 사용하는 인라인 뷰(Inline View)와 SELECT문에 서브쿼리를 사용하느 스칼라 서브쿼리(Scla Subquery)등이 있다.
      - WHERE구에 SELECT문을 사용하면 서브쿼리라고 한다.
     
    ex) 서브쿼리
    ```
    SELECT *
    FROM EMP
    WHERE DEPTNO =
        (SELECT DEPTNO FROM DEPT
          WHERE DEPTNO=10);

    여기서 괄호 안에 잇는 SELECT문을 서브쿼리라고 하면 서브쿼리 밖의 SELECT문은 메인쿼리이다.
    ```

    ex) 인라인뷰
   ```
   SELECT *
   FROM (SELECT ROWNUM NUM, ENAME
         FROM EMP) a
   WHERE NUM <5;

   FROM구에 SELECT문을 사용하여 가상의 테이블을 만드는 효과를 얻을수 있따.
   FROM구에 SELECT문을 사용한 것이 인라인 뷰(Inline View)이다.
   ```

   - 단일 행 서브쿼리와 다중 행 서브쿼리
     - 서브쿼리는 반환하는 행 수가 한 개인 것고 ㅏ여러개인 것에 따라서 단일 행 서브쿼리와 멀티 행 서브쿼리로 분류된다.
     - 단일 행 서브쿼리는 단 하나의 행만 반환하는 서브쿼리로 비교 연산자를 사용한다.
     - 다중 행 서브쿼리는 여러 개의 행알 반환하는것으로 IN,ANY,ALL,EXISTS를 사용해야 한다.
    
   - 다중 행(Multi row) Subquery
     - 여러 개의 행을 반환하는 것으로 다중 행 연산자를 사용해야 한다.
    
     - 다중 행 비교 연산자
       - IN(Subquery) : Main query의 비교조건이 Subquery의 결과 중 하나만 동일하면 참이 된다(OR 조건)
       - ALL(Subquery) : Main query와 Subquery의 결과 모두 동일하면 참이 된다.
                         <ALL : 최솟값을 반환한다.
                         >ALL : 최댓값을 반환한다.
       - ANY(Subquery) : Main query와 Subquery의 결과 중 하나 이상 동일하면 참이 된다.
                         <ANY : 하나라도 크게 되면 참이 된다.
                         >ANY : 하나라도 작게 되면 참이 된다.
       - EXISTS(Subquery) : Main query와 Subquery의 결과가 하나라도 존재하면 참이 된다.
      
         ex) IN
          - 반환되는 여러 개의 행 중에서 하나만 참이 되어도 참이 되는 연산이다.
           ```
           SELECT ENAME, DNAME, SAL
             FROM EMP, DEPT
           WHERE EMP.DEPTNO = DEPT.DEPTNO
             AND EMP.EMPNO
             IN(SELECT EMPNO FROM EMP
                 WHERE SAL>2000);

           EMP테이블과 DEPT테이블의 컬럼 DEPTNO으로 연결하여 ENAME, DNAME, SAL의 정보를 조회한후,
           EMP의 테이블에서 SAL값이 2000 초과하는 값을 가진 행의 EMPNO컬럼을 조회하여
           EMPNO와 일치하는 행의 "ENAME, DNAME, SAL" 값을 조회해라 

           ```
         ex) ALL
          - 메인쿼리와 서브쿼리의 결과가 모두 동일하면 참이된다.
           ```
           SELECT *
           FROM EMP
           WHERE DEPTNO <= ALL(20,30);

           EMP 테이블에서 DEPTNO의 컬럼값이 20,30보다 작거나 같으면 조회해라
           ```
           
         ex) EXISTS
          - 서브쿼리로 어떤 데이터 존재 여부를 확인하는 것이다.
          - 즉 EXISTS의 결과는 참과 거짓이 반환된다.
          ```
          SELECT ENAME, DNAME, SAL
            FROM EMP, DEPT
          WHRE EMP.DEPTNO = DEPT.DEPTNO
            AND EXISTS (SELECT 1 FROM EMP
                        WHERE SAL >2000);
          ```


    - 스칼라(Scala) Subquery
      - 스칼라 Subquery는 반드시 한 행과 한 컬럼만 반환하는 서브쿼링다.
      - 이때 여러 행이 반환되면 오류가 일어난다.
        ```
        SELECT ENAME AS "이름",
                SAL AS "급여",
                (SELECT AVG(SAL)
                  FROM EMP
                )AS "평균급여"
        FROM EMP
        WHERE EMPNO =1000;

        EMP 테이블에서 SAL칼럼의 평균값을 계산한 후에 컬럼이름을 "평균급여"라고 지칭한 후
        EMP 테이블에서 EMPNO의 값이 1000인 행을 조회하여 ENAME(이름),SAL(급여), "평균급여" 3개의 칼럼과 1개의 행을 가진 테이블을 반환한다.
        ```
        
    - 연관(Correlated) Subquery
      - 서브쿼리내에서 메인쿼리 내의 칼럼을 사용하는 것을 의미한다.
        ```
        FROM EMP a
        WHERE a.DEPTNO =
              (SELECT DEPTNO FROM DEPT b
               WHERE b.DEPTNO = a.DEPTNO);

        EMP 테이블을 기준으로 DEPT테이블의 DEPTNO의 컬럼과 EMP테이블의 DEPTNO의 컬럼을 비교하여 동일한 것을 행을 반환한다.
        ```
    
    
2. 조인(JOIN)
   - EQUI(등가) JOIN(교집합)
     1)EQUI(등가) JOIN
         - 여러개의 릴레이션을 사용해서 새로운 릴레이션을 만드는 과정
         - 기본은 교집합을 만드는것이다.
         - '='기호를 사용하여 테이블을 연결한다.

    ![image](https://github.com/user-attachments/assets/e58a192d-c4fc-4703-a85a-f3931cc2ec7a)


       ex)
       ```
        SELECT *
        FROM EMP, DEPT
        WHERE EMP.DEPTNO = DEPT.DEPTNO
            AND EMP.ENAME LIKE '임%'
            ORDER BY ENAME

        EMP, DEPT의 테이블에서 각 컬럼 DEPTNO를 기준으로 각각의 행을 이어 붙인후 ENAME칼럼에 '임'이 들어간 행을 오름차순으로 반환한다.
       ```
    EQUI JOIN을 한후에 실행 계획을 확인해 보면 다음의 순서와 같다.
     1. DEPT테이블과 EMP 테이블 전체를 읽은 후 다음에(TABLE ACCESS FULL) 해시함수를 사용해서 2개의 테이블을 연결한다.
     2. 해시 함수는 테이블을 해시 메모리에 적재한 후에 해시 함수로써 연결한다.

  TIP - 해시조인(HASH JOIN)
       - 먼저 선행 테이블을 결정하고 선행 테이블에서 주어지 조건(Where구)에 해당하는 행을 선택한다.
       - 해당 행이 선택되면 조인 키(Join Key)를 기준으로 해서 해시 함수를 사용해서 해시 테이블을 메인 메모리(Main Memory)에 생성하고 후행 테이블에서 주어진 조건에 만족하는 행을 찾는다.
       - 후행 테이블의 조인 키를 사용해서 해시 함수를 적용하여 해당 버킷을 검색한다.
        
     2) INNER JOIN
        - ON 문을 사용하여 테이블을 연결한다.
          ```
          SELECT *
          FROM EMP INNER JOIN DEPT
          ON EMP.DEPTNO =DEPT.DEPTNO
          AND EMP.ENAME LIKE '임%'
          ORDER BY ENAME;

          EMP 테이블을 기준으로 EMP의 DEPTNO컬럼과 DEPT테이블의 DEPTNO컬럼을 연결하여 EMP의 ENAME의 값이 '임'이 들어가있는 행을 오름차순으로 반환해라
          ```
     3) INTERSECT연산
        - 2개의 테이블에서 교집합을 조회한다.
        - 공통된 값을 조회
        ```
        SELECT DEPTNO
        FROM EMP
        INTERSECT
            SELECT DEPTNO FROM DEPT;

        EMP 테이블의 DEPTNO컬럼을 기준으로 DEPT테이블의 DEPTNO의 컬럼과 비교하여 공통된 값을 조회한다.
        ```

   - NON-EQUI(비등가) JOIN
     - 2개의 테이블 간에 조인하는 경우 "="을 사용하지 않고 ">","<",">=",'"<=" 등을 사용한다.
     - 즉 정확하게 일치 하지 않는것을 조인한다.
       
   - OUTER JOIN
     - 2개의 테이블 간에 교집합을 조회하고 한쪽 테이블에만 있는 데이터도 포함시켜서 조회한다.
       ```
       SELECT *
       FROM DEPT, EMP
       WHERE EMP.DEPTNO (+)=DEPT.DEPTNO;

       테이블 DEPT와 EMP를 서로 비교하여 EMP테이블을 기준으로 DEPTNO의 값을 가진것들을 반환 할떄 EMP에는 없지만 DEPT에 있는 DEPTNO의 값도 같이 반환한다.

       ```
       ![image](https://github.com/user-attachments/assets/951a27a1-6952-483a-a575-e41819176ee0)



     - LEFT OUTER JOIN 과 RIGHT OUTER JOIN
       1. LEFT OUTER JOIN
          ```
          SELECT *
          FROM DEPT LEFT OUTER JOIN EMP
          ON EMP.DETNO = DEPT.DEPTNO;

         왼쪽에 있는 DEPT를 기준으로 조회
          ```

       2. RIGHT OUTER JOIN
          ```
          SELECT *
          FROM DEPT RIGHT OUTER JOIN EMP
          ON EMP.DEPTNO = DEPT.DEPTNO;
    
          오른쪽에 있는 EMP를 기준으로 조회
          ```
       
   - CROSS JOIN
     - 조건구 없이 2개의 테이블을 하나로 조인한다.
     - 조인구가 없기 때문에 카테시안 곱이 발생한다.
     - 행이 14개 있는 테이블과 행이 4개 있는 테이블을 조인하면 56개(14*4)의 행이 조회된다.
     - FROM 절에 "CROSS JOIN" 구를 사용하면 된다.
       ```
       SELECT *
       FROM EMP CROSS JOIN DEPT;

       or

       SELECT *
       FROM EMP,DEPT
       ```
    
   - UNION을 사용한 합집합 구현
     1) UNION
     - 2개의 테이블을 하나로 만드는 연산이다.
     - 두개의 테이블의 칼럼 수 ,칼럼의 데이터 형식 모두가 일치해야한다. 만약 다를시 오류가 발생한다.
     - 합치면서 중복 데이터는 제거된다. 그리고 정렬(SORT)과정을 발생시킨다.
       ```
       SELECT DEPTNO FROM EMP
       UNION
       SELECT DEPTNO FROM EMP
       ```
     2) UNION ALL
     - 2개의 테이블을 하나로 합친다.
     - UNION과는 다르게 중복 제거, 정렬을 유발하지 않는다.
    
       ```
       SELECT DEPTNO FROM EMP
       UNION ALL
       SELECT DEPTNO FROM EMP
       ```
       
   - 차집합을 만드는 MINUS
     - 2개의 테이블에서 차집합을 조회한다. 즉 먼저 쓴 SELECT문에는 있고 뒤에 쓰는 SELECT문에는 없는 집합을 조회한다.
       ```
       SELECT DEPTNO FROM DEPT
       MINUS
       SELECT DEPTNO FROM EMP;

       DEPT 테이블의 DEPTNO컬럼과 EMP 테이블의 DEPTNO컬럼 2개를 비교해서 DEPT 테이블만 가지고 있는 값만 반환한다.
       ```

3. 그룹함수(Group Function)
    - ROLLUP
      - GROUP BY의 칼럼에 대해서 Subtotal을 만들어 준다.
      - ROLLUP 할때 GROUP BY구에 컬럼이 2개 이상 오면 순서에 따라서 결과가 달라진다.
        ```
        SELECT DECODE(DEPTNO, NULL, '전체합계', DEPTNO), SUM(SAL)
        FROM EMP
        GROUP BY ROLLUP(DEPTNO);

        DECODE함수 => DEPTNO의 값이 NULL이면 '전체합계'라는 텍스트를, NULL이 아니라면 DEPTNO라는 텍스트를 반환한다.
        SAL의 값을 모두 합한다.
        EMP 테이블에서 DEPTNO의 값으로 구룹화 하여 합계를 새로운 행을 만들어 반환한다.
        새로운 행을 만드게 되면 DEPTNO의 값은 당연하게 NULL이기 때문에 '전체합계'라는 텍스트가 된다.
        ```
     ![image](https://github.com/user-attachments/assets/03de5537-083b-44a4-b0df-dab30c2ea5f7)

      ```
      SELECT DEPTNO, JOB, SUM(SAL)
      FROM EMP
      GROUP BY ROLLUP (DEPTNO, JOB);

      EMP테이블에서 DETNO, JOB 별로, 전체합계가 모두 죄횐 된다.

      ```
![image](https://github.com/user-attachments/assets/7a3a7290-5521-4b6d-bfd8-d582223fbe56)


    - GROUPING함수
     - ROLLUP, CUBE, GROUPING SETS에서 생성되는 합계값을 구분하기 위해서 만들어진 함수이다.
     - 소계, 합계 등이 계산되면 GROUPING 함수는 1을 반환하고 그렇지 않으면 0을 반환해서 합계값을 식별할수 있다.
     ```
     SELECT DEPTNO, GROUPING(DEPTNO),
         JOB, GROUPING(JOB), SUM(SAL)
     FROM EMP
     GROUP BY ROLLUP(DEPTNO,JOB);

     ```
![image](https://github.com/user-attachments/assets/081c5772-124b-476e-9808-67bda7e2dfb1)

    - GROUPING의 반환값을 통해 구분을 하게된다. 보고서 형식의 쿼리를 작성할때 유용하다.

    ```
    SELECT DEPTNO,
     DECODE(GROUPING(DEPTNO),1,'전체합계') TOT, 
     JOB,
     DECODE(GROUPING(JOB),1,'부서합계') T_DEPT,
     SUM(SAL)
    FROM EMP
    GROUP BY ROLLUP(DEPTNO, JOB);

    EMP 테이블의 DEPTNO컬럼을 조회해서
    TOT이름의 컬럼으로 DEPTNO의 GROUPING값이 1이면 '전체합계'를 아니면 null을 반환하며
    T_DEPT이름의 컬럼으로 JOB의 GROUPING값이 1이면 '부서합계'를 아니면 null을 반환하며
    DEPTNO, JOB를 구룹화한 행에 SUM(SAL)값을 반환해라
    ```
![image](https://github.com/user-attachments/assets/9a15b0ba-536f-4503-8e92-06f9d6d60b45)
     
    - GROUPING SETS 함수
     - GROUP BY에 나오는 칼럼의 순서와 관계없이 다양한 소계를 만들수 있다.
     - GROUP BY에 나오는 칼럼의 순서와 관계없이 개별적으로 모두 처리한다.
     ```
     SELECT DEPTNO, JOB,SUM(SAL)
     FROM EMP
     GROUP BY GROUPING SETS(DEPTNO,JOB);

     EMP 테이블에서 DEPTNO, JOB< SUM(SAL)컬럼을 조회하는데 DEPTNO,JOB컬럼 가각 그룹화하여 합계를 반환해라
     ```

     
    - CUBE 함수
     - CUBE함수에 제시한 칼럼에 대해서 결합 가능한 모든 집계를 계산한다.
     - 다차원 집계를 제공하여 다양하게 데이터를 분석할수 있게 한다.
     - 조합할 수 있는 경우의 수가 모두 조합된다.
     ```
     SELECT DEPTNO, JOB, SUM(SAL)
     FROM EMP
     GROUP BY CUBE(DEPTNO, JOB);

    EMP테이블에서 전체합계, DEPTNO 합계, JOB 합계 DEPTNO-JOB합계 모두 조회된다.
     ```
    ![image](https://github.com/user-attachments/assets/47e62384-5cb6-4951-8a48-6ea17f2725a0)


3. 윈도우 함수(Window Function)
 3-1. 윈도우 함수
 - 행과 행 간의 관계를 정의하기 위해서 제공되는 함수
 - 순위, 합계, 평균, 행 위치 등을 조작할수 있다.

   - 윈도우 함수 구조
   ```
   SELECT WINDOW_FUNCTION(ARGUMENTS)
           OVER([PARTITION BY 칼럼]
                   [ORDER BY] [WINDOWING절])
   FROM 테이블명;
   - WINDOW_FUNCTION : 함수명 ( SUN, MAX, MIN, RANK .... )
   - OVER : 분석함수임을 표시 
   - PARTITION : 대상을 그룹화할 기준
   - ORDER BY : 그룹에 대한 정렬 기준
   - WINDOWING 절 : 분석함수의 대상이 되는 범위를 지정, ORDER BY 절에 종속적, 
                    [ 기본 생략/DEFAULT ] 로 range between unbounded preceding and current row 실행
                    -> 정렬된 결과의 처음부터 현재 행 까지
   ```

   ● ARGUMENTS(인수) : 윈도우 함수에 따라서 0~N개의 인수를 설정한다.
   ● PARTITION BY : 전체 집합을 기준에 의해 소그룹으로 나누다.
   ● ORDER BY : 어떤 항목에 대해서 정렬한다.
   ● WINDOWING : 행 기준의 범위를 정한다. ROWS는 물리적 결과의 행 수이고 RANGE는 논리적인 값에 의한 범위이다.

   - WINDOWING
    ● ROWS : 부분적인 윈도우 크기를 물리적 단위로 행의 집합을 지정한다.
    ● RANGE : 논리적인 주소에 의해 행 집합을 지정한다.
    ● BETWEEN ~ AND : 윈도우의 시작과 끝의 위치를 지정한다.
    ● UNBOUNDED PRECEDING : 윈도우의 시작 위치가 첫 번째 행임을 의미한다.
    ● UNBOUDED FOLLOWING : 윈도우 마지막 위치가 마지막 행임을 의미한다.
    ● CURRENT ROW : 윈도우 시작 위치가 현재 행임을 의미한다.


   ex) WINDOWING
   ```
   SELECT EMPNO, ENAME, SAL,
   SUM(SAL) OVER(ORDER BY SAL
                   ROWS BETWEEN UNBOUNDED PRECEDING   -> 첫번째 행을 의미
                   AND UNBOUNDED FOLLOWING) TOTSAL     -> 마지막 행을 의미
   FROM EMP;

   EMP 테이블을 조회하는데 EMPNO, ENAME, SAL 칼럼과 SUM함수를 통해 첫행부터 마지막 행까지 오름차순으로 정렬한 값을 TOTSAL 컬럼에 반환 => SAL의 전체 합계를 조회한다.
   ```
   ex) WINDOWING-1
   ```
   SELECT EMPNO, ENAME, SAL
   SUM(SAL) OVER(ORDER BY SAL
                   ROWS BETWEEN UNBOUNDED PRECEDING
                   AND CURRENT ROW) TOTSAL

    EMP 테이블을 조회하는데 EMPNO, ENAME, SAL 칼럼과 SUM함수를 통해 첫행부터 현재 행까지 오름차순으로 정렬한 값을 TOTSAL 컬럼에 반환 => 행 별로 누적 합계 계산
   ```

 3-2. 순위함수(RANK Function)
  - 특정 항목과 파티션에 대해서 순위를 계산할수 잇는 함수를 제공
  - RANK, DDENSE_RANK, ROW_NUMBER 함수 있다.

    - 순위(RANK) 관련 윈도우 함수
    ● RANK : 특정항목 및 파티션에 대해서 순위를 계산한다. 동일한 순위는 동일한 값이 부여된다.
    ● DENSE_RANK : 동일한 순위를 하나의 건수로 계산한다.
    ● ROW_NUMER : 동일한 순위에 대해서 고유의 순위를 부여한다.
   ex)
   ```
   SELECT ENAME, SAL,
            RANK() OVER (ORDER BY SAL DESC) ALL RANK,
            RANK() OVER (PARTITION BY JOB ORDER BY SAL
                        DESC) JOB RANK
   FROM EMP;

   EMP테이블에서 ENAME, SAL 칼럼을 가지고 온후 SAL칼럼의 값을 순환하여 내림차순으로 정렬후 ALL RANK 칼럼을 만든곳에 반환한 후,
                                               JOB의 칼럼으로 소그룹을 만든 후 SAL칼럼의 값을 순환하여 내림차순으로 정렬 후 JOB RANK칼럼에 값을 반환한다.
   ```

ex2)
![image](https://github.com/user-attachments/assets/0c77a7f5-93d4-4b07-b0d2-d4fdde02d7cc)

3-3 집계 합수(AGGERGATE Function)
 - 윈도우 함수를 제공한다.
    - 집계(AGGERGATE)관련 윈도우 함수
    ● SUM : 파티션별로 합계를 계산
    ● AVG : 파티션별로 평균 계산
    ● COUNT : 파티션별로 행 수를 계산
    ● MAX와MIN : 파티션별로 최댓값과 최솟값 계산
   

4. Top N쿼리
5. 계층형 조회(CONNECT BY)
6. PIVOT과 UNPIVOT
7. 테이블 파티션(Table Partition)
8. 정규표현식(Regular Expression)
 
