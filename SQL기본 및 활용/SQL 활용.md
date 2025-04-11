


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




       
   - CROSS JOIN
   - UNION을 사용한 합집합 구현
   - 차집합을 만드는 MINUS
     








2. 그룹함수(Group Function)
3. 윈도우 함수(Window Function)
4. Top N쿼리
5. 계층형 조회(CONNECT BY)
6. PIVOT과 UNPIVOT
7. 테이블 파티션(Table Partition)
8. 정규표현식(Regular Expression)
 
