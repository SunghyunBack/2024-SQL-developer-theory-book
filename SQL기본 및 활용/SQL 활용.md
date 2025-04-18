
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


4. 윈도우 함수(Window Function)
 4-1. 윈도우 함수
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

 4-2. 순위함수(RANK Function)
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

4-3 집계 합수(AGGERGATE Function)
 - 윈도우 함수를 제공한다.
    - 집계(AGGERGATE)관련 윈도우 함수
    ● SUM : 파티션별로 합계를 계산
    ● AVG : 파티션별로 평균 계산
    ● COUNT : 파티션별로 행 수를 계산
    ● MAX와MIN : 파티션별로 최댓값과 최솟값 계산
   ex)
   ```
   SELECT ENAME, SAL
    SUM(SAL) OVER (PARTITION BY MGR) SUM MGR
   FROM EMP;

   EMP 테이블에서 ENMA,SAL을 조회하는데 SUM MGR이라는 컬럼의 이름을 생성한 뒤 MGR칼럼의 값으로 그룹화 하여 SUM(SAL)값을 계산한후 반환한다.
   ```

4-4 행 순서 관련 함수
 - 상위 행의 값을 하위에 출력하거나 하위 행의 값을 상위 행에 출력할수 있다.
 - 특정 위치의 행을 출력할수 있다.
   
    - 행 순서 관련 윈도우 함수
    ● FIRST_VALUE : 파티션에서 가장 처음에 나오는 값을 구한다. MIN함수를 사용해서 같은 결과를 구할수 있다.
    ● LAST_VALUE : 파티션에서 가장 나중에 나오는 값을 구한다. MAX함수를 사용해서 같은 결과를 구할수 있다.
    ● LAG : 이전 행을 가지고 온다.
    ● LEAD : 윈도우에서 특정 위치의 행을 가지고 온다. 기본값은 1이다.
   
   ex) FIRST_VALUE
   ```
   SELECT DEPTNO, ENAME, SAL,
   FIRST_VALUE(ENAME) OVER (PARTITION BY DEPTINO
    ORDER BY SAL DESC ROWS UNBOUNDED PRECEDING) AS DEPT_A
   FROM EMP;

   EMP 테이블에서 DEPTNO, ENAME, SAL 조회한후 ENAME의 첫번째 행을 가지고 온 후 DEPTNO 파티션을 만들고 SAL을 기준으로 내림차순한 후 DEPT_A 컬럼의 첫번째 행으로 반환해라
   ```
   ![image](https://github.com/user-attachments/assets/1c90f041-d78b-4036-be92-3f54dd4d93f4)


   ex) LAST_VALUE
   ```
   SELECT DEPTNO, ENAME, SAL,
   LAST_VALUE(ENAME) OVER (PARTITION BY DEPTINO
    ORDER BY SAL DESC ROWS UNBOUNDED CURRENT ROW AND UNBOUNDED FOLLOWING) AS DEPT_A
   FROM EMP;

   EMP 테이블에서 DEPTNO, ENAME, SAL을 조회한 후 ENAME의 마지막 행을 가지고 와서 EPTINO파티션을 만들고  현재행에서 마지막 행까지 조회한후SAL을 기준으로 내림차순한 현재의 값을  DEPT_A 컬럼의 첫번째 행으로반환해라
   ```
   ![image](https://github.com/user-attachments/assets/168c2f5f-8bc2-492d-896f-e59311715223)

   ex) LAG
   ```
   SELECT DEPTNO, ENAME, SAL,
   LAG(SAL) OVER(ORDER BY SAL DESC) AS PRE_SAL
   FROM EMP;

   EMP테이블에서 DEPTNO, ENAME, SAL 조회한후 PRE_SAL칼럼에 SAL값을 기준으로 내림차순한 후 각 행의 이전값을 현재 행에 값을 반환해라
   ```
   ![image](https://github.com/user-attachments/assets/f286d0e4-da19-4bee-8797-dc23c925be1e)

   ex) LEAD
   ```
   SELECT DEPTNO, ENAME, SAL,
   LEAD(SAL,2) OVER(ORDER BY SAL DESC) AS PRE_SAL
   FROM EMP;

   EMP 테이블에서 DETNO, ENAME, SAL 조회한 후 PRE_SAL 칼럼에 SAL값을 기준으로 내림차순한 후 각 행을 기준으로 2번째 이후의 값을 반환해라
   ```
![image](https://github.com/user-attachments/assets/7e02f3aa-1466-4a63-b56e-33015bd2ddff)

4-5 비율 관련 함수
 - 누적 백분율, 순서별 백분율, 파티션을 N분으로 분할한 결과 등을 조회할수 있다.
   
    - 비율 관련 윈도우 함수
    ● CUME_DIST : 파티션 전체 건수에서 현재 행보다 작거나 같은 건수에 대한 누적 백분율을 조회한다. 누적 분포상에 위치를 0~1사이의 값을 가진다.
    ● PERCENT_RANK : 파티션에서 제일 먼저 나온 것을 0으로 제일 늦게 나온 것을 1로 하여 값이 아닌 행의 순서별 백분율을 조회한다.
    ● NTILE : 파티션별로 전체 건수를 ARGUMENT값으로 N등분한 결과를 조회한다.
    ● RATIO_TO_REPORT : 파티션 내에 전체 SUM(칼럼)에 대한 행 별 칼럼값의 백분율을 소수점 까지 조회한다.
ex)
```
SELECT DEPTNO, ENAMS, SAL,
PERCENT_RANK() OVER (PARTITION BY DEPTNO
    ORDER BY SAL DESC) AS PERCENT_SAL
FROM EMP;

EMP테이블에서 DEPTNO, ENAMS, SAL 조회한 후 DEPENO를 그룹화 하여 SAL로 내림차순 한 것을 PERCENT_SAL의 컬럼에 백분율로 반환한다.
```
![image](https://github.com/user-attachments/assets/dd09fc22-3900-480a-a445-f30f5ce2a9b3)

```
SELECT DEPTNO, ENAME, SAL,
 NTILE(4), OVER(ORDER BY SAL DESC) AS N_TILE
FROM EMP;

EMP테이블에서 DEPTNO, ENAMS, SAL 조회한 후 SAL 값을 내림차순으로 정렬하고 N_TILE컬럼에 4개로 등분하여 분류한후 반환한다.
```
![image](https://github.com/user-attachments/assets/d38c25df-8020-43c1-a422-347bda4e3a16)


4. Top N쿼리
 4-1. ROWNUM
   - ORACLE 데이터베이스의 SELECT문 결과에 대해서 논리적인 일련번호를 부여한다.
   - 조회되는 행 수를 제한할대 많이 사용된다.
   - 환며에 데이터를 출력할 때 부여되는 논리적 순번이다. 페이지 단위 출력을 하기 위해서는 인라인뷰(Inline View)를 사용해야한다.

ex)
```
SELECT *
FROM EMP
WHERE ROWNUM <=1;

EMP 테이블을 조회하는데 1이하의 행을 조회한다.
```

ex) 인라인 뷰 (FROM 절에 SELECT문을 사용하는 경우)
```
SELECT *
FROM (SELECT ROWNUM list, ENAME FROM EMP)
WHERE list<=5;

EMP테이블에서 ENAME과 LIST를를 조회할때 List를 기준으로 논리적인 일련번호를 부여해서 조회한수 list가 5개 이하의 행을 반환한다.
```

 4-2. ROWID
   - ORACLE 데이터베이스 내에서 데이터를 구분할 수 있는 유일한 값이다.
   - "SELECT ROWID, EMPNO FROM EMP"와 같은 SELECT문으로 확인할수 있다.
   - 데이터가 어떤 데이터 파일, 어느 블록에 저장되어 있는지 알수 있다.

●ROWID구조
|구조|길이|설명|
|---|---|---|
|오브젝트 번호|1~6|오브젝트(Object)별로 유일하 ㄴ값을  가지고 잇으며, 해당 오브젝트가 속해 있는 값이다.|
|상대파일 번호|7~9|테이블스페이스(Tablespace)에 속해있는 데이터 파일ㅇ에 대한 상대 파일번호이다.|
|블록 번호|10~15|데이터 파일 내부에서 어느 블록에 데이터가 있는지 알려준다.|
|데이터 번호|16~18|데이터블록에 데이터가 저장되어 잇는 순서를 의미한다.|

ex)
```
SELECT ROWID, winetypename
FROM winetype;

모든 테이블은 ROWID를 가지고 있다.
```

6. 계층형 조회(CONNECT BY)
 - ORACLE 데이터베이스에서 지원하는 것으로 계층형으로 데이터를 조회할수 있따.

예제

![image](https://github.com/user-attachments/assets/fc84aa01-1c72-4940-8484-3d808e1068da)

ex)
```
SELECT MAX(LEVEL)
FROM Limbest.EMP
START WITH MGR IS NULL
CONNECT BY PRIOR EMPNO = MGR;
```
※참고사항
- CONNECT BY는 트리(TREE)형태의 구조로 질의를 수행하는 것이며 START WITH구는 시작 조건을 의미하고 CONNECT BY PRIOR는 조인조건이다.
 MAX(LEVEL)을 사용하여 최대 계층 수를 구할수 있다. 
- PRIOR 키워드 : 바로직전에 출련된 행을 의미한다.

● CONNECT BY 구조
|키워드|설명|
|---|---|
|LEVEL|검색 항목의 깊이를 의미한다. 즉, 계층구조에서 가장 상위 레벨이 1이된다.|
|CONNECT_BY_ROOT|계층 구조에서 가장 최상위 값을 표시한다.|
|CONNECT_BY_ISLEAF|계층구조에서 갖아 최하위를 표시한다.|
|SYS_CONNECT_BY_PATH|계층구조의 전체 전개 경로를 표시한다.|
|NOCYCLE|순환 구조가 발생지점까지만 전개된다.|
|CONNECT_BY_ISCYCLE|순환 구조 발생 지점을 표시한다.|

※참고사항
 - CONNECT BY구는 순방향 조회와 역방향 조회가 있다.
 - 순방향 : 부모 엔티티로 부터 자식 엔티티를 찾아간다.
 - 역방향 : 자식 엔티티로 부터 부모 엔티티를 찾아간다.


● 계층현 조회
|키워드|설명|
|---|---|
|START WITH 조건|계층 전개의 시작 위치를 지정하는것이다.|
|PRIOR 자식 = 부모|부모에서 자식 방향으로 검색을 수행하는 순방향 전개이다.|
|PRIOR 부모 = 자식|자식에서 부모방향으로 검색을 수행하는 역방향 전개이다.|
|NOCYCLE|데이터를 전개하면서 이미 조회된 데이터를 다시 조회되면  CYCLE이 형성된다. 이때 NOCYCLE은 사이클이 발생되지 않게 한다.|
|ORDER SIBLINGS BY 칼럼명|동일한 LEVEL인 형제노드 사이엥서 정령을 수행한다.|

ex)순방향
```
SELECT LEVEL, LPAD(' ', 4*(LEVEL-1)) || EMPNO,
        MGR, CONNECT_BY_ISFEAF ISLEAF
FROM EMP
START WITH MGR IS NULL
CONNECT BY PRIOR EMPNO=MGR;
```

ex)역방향
```

SELECT LEVEL, LPAD(' ', 4*(LEVEL-1)) || EMPNO,
        MGR, CONNECT_BY_ISFEAF ISLEAF
FROM EMP
START WITH EMPNO = 1008
CONNECT BY PRIOR MGR = EMPNO;
```

7. PIVOT과 UNPIVOT
 - 테이블에서 하나의 칼ㄹ럼에 잇는 행 값들을 펼쳐 각각을 하나의 칼럼으로 만들어 주는것이다.
 - 데이터를 행기반에서 열기반으로 바꾸는 것이 PIVOT이고 다시 열기반에서 행기반으로 바꾸는것이 UNPIVOT이다.

ex)
```
SELECT 상품명,[2023],[2024]
FRON(SELECT 상품명, 수량, 주문연도 FROM주문) AS D
    PIVOT(SUM(수량)FROM 주문연도 IN ([2023],[2024]) ) AS PVT;
```

![image](https://github.com/user-attachments/assets/548a8c90-edb8-4bd6-99ba-9eb82cf0ab8d)



8. 테이블 파티션(Table Partition)
 8-1. Partiton 기능
 - 대용량 테이블을 여러개의 데이터 파일에 분리해서 저장한다.
 - 테이블의 데이터가 물리적으로 분리된 데이터 파일에 저장되면 입력, 수정, 삭제, 조회 성능이 향상된다.
 - 파티션은 각각의 파티션 별로 독립적으로 관리될수 있다. 즉 파티션 별로 백업, 복구, 전용 인덱스 생성이 가능하다.
 - ORACLE 데이터베이스의 논리적 관리 단위인 테이블 스페이스 간에 이동이 가능하다.
 - 데이터를 조회할 때 데이터의 범위를 줄여서 성능을 향상시킨다.

 8-2. Range Partition
  - 테이블의 칼럼 중에서 값의 범위를 기준으로 여러개의 파티션으로 데이터를 나누어 저장하는 것이다.

 8-3. List Partition
 - 특정 값을 기준으로 분할하는 방법이다.

 8-4. Hash Partition
 - 내부적으로 해시 함수를 사용해서 데이터를 분할한다.
 - 결과적으로 데이터베이스 관리시스템이 알아서 분할하고 관리한다.

 8-5. 파티션 인덱스
 - 4가지 유형의 인텍스를 제공한다. 즉 파티션 키를 사용해서 인덱스를 만드는 Prefixed Index와 해당 파티션만 사용하는 Local Index 등으로 나누어 진다.
 - Oracle 데이터베이스는 Global Non-Prefixed를 지원하지 않는다.

● 파티션 인덱스(Partition Index)
|구분|중요내용|
|---|---|
|Global Index|여러 개의 파티션에서 하나의 인덱스를 사용한다.|
|Local Index|해당 파티션 별로 각자의 인덱스를 사용한다.|
|Prefixed Index|파티션 키와 인덱스 키가 동일하다.|
|Non Prefixed Index|파티션 키와 인덱스 키가 다르다.|

9. 정규표현식(Regular Expression)
 - 특정한 규칙을 가지고 있는 문자열 집합을 표현하기 위해서 사용되는 형식 언어이다.
 - 전화번호, 주민등록번호 등 특정 규칙을 가지고 잇는 데이터를 찾을 때 사용가능하다.
 - SQL에서는 'regexp'를 사용하면 된다.

● ORACLE 정규 표현식
|구분|중요내용|
|---|---|
|REGEXP_LIKE|LIKE문과 유사하고 정규표현식을 검색한다.|
|REGEXP_REPLACE|정규표현식을 검색한 후에 문자열을 변경한다.|
|REGEXP_INSTR|정규표현식을 검색하고 위치를 반환한다.|
|REGEXP_SUBSTR|정규표현식을 검색하고 문자열을 추출한다.|
|REGEXP_COUNT|정규표현식을 검색하고 발견된 횟수를 반환한다.|

● 정규 표현식을 사용하기 위한 메타문자
|구분|의미|
|---|---|
|.|임의의 한 문자이다.|
|?|앞 문자가 없거나 하나 있음을 의미한다(0또는1번 발생).|
|+|앞 문자가 하나 이상 있음을 의미한다.|
|*|앞 문자가 0개 이상있음을 의미한다.|
|{m}|선행표현식이 정확히 m번 발생한다.|
|{m,}|선행 표현식이 최소 m번 이상 발생한다.|
|{m,n}|선형 표현식이 최소 m번이상, 최대 n번 이하 발생한다.|
|[...]|괄호 안의 리스트에 있는 임의의 단일 문자와 일치한다.|
|"|"| OR을 의미한다.|
|^|문자열 시작 부분과 일치한다.|
|[^]|해당 문자에 해당하지 않는 한 문자이다.|
|$|문자열의 끝 부분과 일치한다.|
|\| 표현식에서 후속 문자를 일반문자로 처리한다.|
|\n|괄호 안에 그룹화된 n번째(1-9)선행ㅎ하위식과 일치한다.|
|\d|숫자문자이다.|
|[:class]|지정된 POSIX문자 클래스에 속한 임의의 문자와 일치한다. ex: ["alpha:"]["digit:"]등등|
|[^:class]|괄호안의 리스트에 없는 임의의 단일 문자와 일치한다.|
